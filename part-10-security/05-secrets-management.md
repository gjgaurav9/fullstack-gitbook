# Secrets management

## Why this matters

It's a Tuesday afternoon. A junior engineer pushes a hotfix branch to GitHub. Forty seconds later — before they've even opened the pull request — an automated bot has cloned the repo, scraped the commit, and started spinning up cryptocurrency miners on the AWS account whose access key sat in a `config.py` they'd committed "just to test something locally." By the time the billing alert fires the next morning, the bill is in the thousands of dollars. The key was live for less than a day. The repo was private for most of it. None of that mattered.

This is among the most common serious security incidents in software, and it is almost entirely preventable. The attackers are not clever; they are automated. Public GitHub, npm, Docker Hub, and pastebin are continuously scanned by bots that watch for the recognizable shapes of credentials — `AKIA...` for AWS, `ghp_...` for GitHub tokens, `sk_live_...` for Stripe — and act within seconds. GitHub's own secret-scanning program partners with credential issuers to detect and revoke leaked tokens, and the window between "pushed" and "exploited" is routinely measured in seconds to minutes, not days.

The expensive misconception is that a secret committed and then deleted is gone. It is not. As the Git internals chapter showed, every commit is an immutable object in `.git/objects/`, reachable from history and from every clone, fork, and reflog. Deleting the file in a later commit leaves the blob sitting in history forever. "We removed it" is not remediation. Rotation is remediation. This chapter is about never getting into that position in the first place, and about what to do — in order — the day you discover you already have.

## Mental model

A secret is any value that grants access and whose disclosure causes harm: database passwords, API keys, signing keys, OAuth client secrets, TLS private keys. The core principle is **separation of the secret from the artifact**. Code is meant to be copied, forked, logged, and shared. Secrets are meant to be tightly held and frequently changed. The moment those two lifecycles touch — a secret baked into code, an image, or a committed `.env` — you've coupled a thing you distribute widely to a thing you must keep narrow.

It helps to think in terms of a **trust boundary**: the line between "things an attacker who reads your source could obtain" and "things they could not." Every artifact you ship — a Git repo, a container image, a CI log, a stack trace in your error tracker, a Slack paste — lives on the public side of that boundary, even when access is nominally restricted. A secret belongs on the other side. The whole discipline of secrets management is keeping the boundary intact: making sure the value that grants access never ends up inside something you copy around freely.

The maturity ladder runs from "embedded in code" (worst) to "injected at runtime from a managed store" to "minted on demand, short-lived, never stored" (best):

```mermaid
flowchart TD
    A[Hardcoded in source<br/>lives in Git history forever] --> B[Committed .env file<br/>same problem different file]
    B --> C[Runtime env var<br/>from CI/CD secret store]
    C --> D[Centralized vault<br/>KMS or Secrets Manager or Vault]
    D --> E[Dynamic short-lived secret<br/>minted per-request auto-expiring]
    style A fill:#7f1d1d,color:#fff
    style B fill:#7f1d1d,color:#fff
    style C fill:#92400e,color:#fff
    style D fill:#166534,color:#fff
    style E fill:#14532d,color:#fff
```

Two properties matter as you climb. **Centralization** means there is one place to grant, audit, and revoke access — you can answer "who can read the production DB password?" and "rotate it now" with a single action, instead of grepping a dozen repos and config maps. **Short-livedness** changes the economics of a leak: a credential that expires in 15 minutes is nearly worthless to an attacker who scrapes it from a log, and rotation becomes a continuous background process rather than a quarterly fire drill that everyone dreads. The endgame is the static long-lived secret that simply doesn't exist to be stolen — the application authenticates with a workload identity and receives a credential that's already expiring as it's issued.

## In practice

### The wrong way, in full

Here is the code that causes the Tuesday incident. A secret hardcoded directly:

```python
# settings.py  — DO NOT DO THIS
DATABASE_URL = "postgres://app:S3cretP@ss@db.prod.internal:5432/main"
STRIPE_KEY = "sk_live_51HxQ2eK8..."
AWS_SECRET_ACCESS_KEY = "wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY"
```

*The same idea in TypeScript:*

```typescript
// settings.ts  — DO NOT DO THIS
export const DATABASE_URL = "postgres://app:S3cretP@ss@db.prod.internal:5432/main";
export const STRIPE_KEY = "sk_live_51HxQ2eK8...";
export const AWS_SECRET_ACCESS_KEY = "wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY";
```

And the slightly-less-obvious version that engineers convince themselves is fine — a committed `.env`:

```bash
$ cat .env            # tracked in Git "for convenience"
DATABASE_URL=postgres://app:S3cretP@ss@db.prod.internal:5432/main
STRIPE_KEY=sk_live_51HxQ2eK8...
$ git add .env && git commit -m "add config"
```

This is the same vulnerability with a different filename. Once committed, the secret is in history on every clone. The fix is not "edit the file later." The fix is to never let it in, and to source the value at runtime.

### Step one: environment hygiene

Keep an `.env.example` with keys but no values, ignore the real `.env`, and load it only in development:

```bash
# .env.example  — committed, documents required keys
DATABASE_URL=
STRIPE_KEY=

# .gitignore
.env
.env.*
!.env.example
```

As the Git internals chapter noted, `.gitignore` only governs files Git doesn't already track. If `.env` was committed before you ignored it, `git rm --cached .env` removes it from the index — but the historical blob still exists, so a previously-committed secret must still be rotated. This is the single most-filed "why isn't my secret file ignored?" support ticket, and the answer is always the same: ignoring is not removing, and removing is not rotating.

### Step two: a managed store

In production, the runtime fetches secrets from a centralized store. With AWS Secrets Manager:

```python
import boto3, json
from functools import lru_cache

@lru_cache(maxsize=None)
def get_secret(name: str) -> dict:
    client = boto3.client("secretsmanager")
    resp = client.get_secret_value(SecretId=name)
    return json.loads(resp["SecretString"])

# The app authenticates via its IAM role (no static AWS keys on the box).
db_url = get_secret("prod/app/db")["url"]
```

*The TypeScript equivalent:*

```typescript
import {
  SecretsManagerClient,
  GetSecretValueCommand,
} from "@aws-sdk/client-secrets-manager";

const client = new SecretsManagerClient({});
const cache = new Map<string, Record<string, string>>();

async function getSecret(name: string): Promise<Record<string, string>> {
  const cached = cache.get(name);
  if (cached) return cached;
  const resp = await client.send(new GetSecretValueCommand({ SecretId: name }));
  const value = JSON.parse(resp.SecretString!);
  cache.set(name, value);
  return value;
}

// The app authenticates via its IAM role (no static AWS keys on the box).
const dbUrl = (await getSecret("prod/app/db")).url;
```

Note what's absent: no AWS key in the code. The instance, container, or Lambda assumes an IAM role via the instance metadata service; the SDK resolves credentials automatically. The only thing the application "knows" is which secret to ask for, not the secret itself. The store enforces access with IAM policy and writes every read to CloudTrail, so you get authorization and an audit log for free.

This sidesteps the **bootstrap problem** that trips up most first attempts: "to read the secret, the app needs a credential — but where does *that* credential come from?" Answer: from the platform, not from a file. The cloud provider injects a short-lived identity (an instance role, a Kubernetes service-account token, an OIDC token) that the SDK exchanges for credentials. There is no root secret on disk to leak, because the first link in the chain is an identity the platform vouches for, not a string you typed.

The same shape works with HashiCorp Vault, where the application authenticates with a role and reads a path:

```bash
# App authenticates (e.g. via Kubernetes service-account JWT), gets a token,
# then reads the secret. No secret is ever written to disk.
$ vault read -field=url secret/data/prod/app/db
postgres://app:S3cretP@ss@db.prod.internal:5432/main
```

### Step three: dynamic, short-lived secrets

The strongest pattern eliminates the stored credential entirely. Vault's database secrets engine generates a *new* database user on each request, scoped and time-bound, and revokes it automatically when the lease expires:

```bash
# Vault creates a fresh Postgres role valid for 1 hour, then drops it.
$ vault read database/creds/app-readonly
Key                Value
---                -----
lease_id           database/creds/app-readonly/h7Yf...
lease_duration     1h
password           A1a-9f2c8e7b6d5a4c3b
username           v-token-app-readonly-x9k2j7
```

There is no master password sitting in a config to leak. This is a categorical improvement over rotation, not merely a faster version of it. Rotation reduces the *time* a leaked secret stays valid; dynamic credentials reduce the *blast radius* and make every credential attributable to a single consumer and a single time window. If a credential is captured, it's dead within the hour, and every issuance is tied to an identity in the audit log — so you learn not just that a leak happened but which workload leaked it and exactly what it could touch. The same engine pattern exists for cloud IAM, SSH, and PKI certificates. AWS offers a comparable model with IAM roles and STS, where `sts:AssumeRole` returns temporary credentials that expire on their own.

### Step four: catch leaks before and after commit

Defense in depth means assuming a secret will eventually slip through and catching it at two gates. A pre-commit hook blocks the secret before it ever becomes a Git object:

```yaml
# .pre-commit-config.yaml  — runs gitleaks on every commit
repos:
  - repo: https://github.com/gitleaks/gitleaks
    rev: v8.18.0
    hooks:
      - id: gitleaks
```

```bash
$ pip install pre-commit && pre-commit install
$ git commit -m "add config"
Detect hardcoded secrets.................................................Failed
- hook id: gitleaks
  Secret detected: aws-access-key in settings.py:3
```

And a CI gate scans the whole history on every push, because pre-commit hooks are local and skippable (`git commit --no-verify`). Server-side enforcement is the backstop:

```yaml
# .github/workflows/secret-scan.yml
name: secret-scan
on: [push, pull_request]
jobs:
  gitleaks:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with: { fetch-depth: 0 }   # full history, not just the tip
      - uses: gitleaks/gitleaks-action@v2
```

Pair this with platform-side scanning — GitHub secret scanning with push protection actively blocks pushes containing recognized credential patterns and notifies partner services to auto-revoke. Turn it on; it costs nothing for public repos and catches what your tooling misses.

### Leaked-credential incident response: rotate first

When a secret leaks, the order of operations is not negotiable, and it is counterintuitive to engineers whose instinct is to clean up the evidence:

1. **Rotate the credential first.** Issue a new secret, deploy it, and revoke the old one. This is the only step that actually stops the bleeding. Cleaning Git history while the key is still live accomplishes nothing — copies already exist.
2. **Assess exposure.** Check access logs (CloudTrail, DB audit logs) for use of the leaked credential from unexpected sources or times. Assume it was scraped the instant it was pushed.
3. **Then, optionally, scrub history** with `git filter-repo` and force-push. This is cosmetic and disruptive (it rewrites every downstream clone), and it is *never* a substitute for rotation. Do it for hygiene after the credential is already dead.

> **Connect the dots:** This rotate-first ordering follows directly from the Git internals chapter (Part 3): a committed secret is an immutable object reachable from history and every clone, so "deleting the file" cannot un-leak it. The same content-addressable durability that makes Git safe for code makes it unforgiving for secrets.

## Pitfalls and anti-patterns

**1. The "scrub it from history and we're fine" reflex.** When someone spots a committed key, the panicked first move is `git filter-repo` and a force-push. This rewrites history but does nothing about the copies that already exist in forks, clones, CI caches, and attacker scrapers. *Recognize it* by the incident channel discussing history rewrites before anyone has rotated. *Fix it* by enforcing the order: rotate and revoke first, investigate exposure second, scrub last and only for hygiene.

**2. Secrets in CI/CD logs and build args.** A pipeline that does `echo $API_KEY` for debugging, or a `Dockerfile` with `ARG SECRET=...`, leaks into build logs and image layers — and Docker layers, like Git objects, are immutable history. *Recognize it* by `docker history <image>` showing a secret in a layer, or a CI log containing a raw value. *Fix it* by using your CI's masked-secret mechanism (GitHub Actions masks registered secrets in logs), build-time secret mounts (`RUN --mount=type=secret`), and never passing secrets as `ARG`.

**3. Over-broad secret access ("one God credential").** A single admin database password shared across every service means a leak from any one service compromises everything, and you can't tell who used it. *Recognize it* by a secret whose IAM policy or Vault path is readable by your entire compute fleet. *Fix it* with least-privilege scoping — per-service identities, per-environment secrets, and dynamic credentials so each consumer gets its own short-lived, attributable grant.

**4. Rotation that nobody can actually perform.** A secret with no documented rotation path becomes load-bearing: everyone is afraid to change it because no one knows what will break. *Recognize it* by a credential that hasn't changed in years and a wiki page titled "DO NOT ROTATE." *Fix it* by building rotation into the design from day one — dual-secret support (accept old and new during a window), automated rotation via Secrets Manager's rotation Lambdas or Vault leases, and a tested runbook.

**5. Trusting `.env` files in production.** Plaintext `.env` on a production host is readable by any process that compromises the box, gets dumped by misconfigured logging or crash handlers, and is easy to accidentally bake into an image. *Recognize it* by SSHing to prod and finding a world-readable `.env`. *Fix it* by injecting secrets from a managed store at process start (or via a sidecar/agent) so they live in memory, not on disk.

## Production checklist

- [ ] No secret values anywhere in source code, including test fixtures and example files
- [ ] `.gitignore` covers `.env` and `.env.*` with an allowlisted `!.env.example` committed for documentation
- [ ] Pre-commit secret scanning installed locally (`gitleaks` / `pre-commit`) for every contributor
- [ ] CI secret scanning on full history (`fetch-depth: 0`) as a required, non-skippable status check
- [ ] Platform secret scanning with push protection enabled (e.g. GitHub Advanced Security)
- [ ] All production secrets sourced at runtime from a managed store (Vault, AWS/GCP/Azure secrets manager or KMS)
- [ ] Workloads authenticate via identity (IAM role, K8s service account, OIDC) — zero static cloud keys on hosts
- [ ] Dynamic/short-lived credentials for databases and cloud access wherever the engine supports it
- [ ] Least-privilege, per-service, per-environment scoping on every secret with an audit log of reads
- [ ] Documented, tested rotation runbook with dual-secret support; automated rotation where possible
- [ ] Incident runbook that orders response as **rotate → assess → scrub**, with on-call trained on it

## Exercises

1. **(Comprehension)** Commit a fake AWS key (`AKIAIOSFODNN7EXAMPLE`) to a throwaway repo, then commit a second change that deletes the file. Run `git log -p` and `git cat-file -p` to demonstrate that the key's blob is still reachable in history. Explain in two sentences why `git rm` followed by a normal commit does not remediate a leak.

2. **(Applied)** Set up `gitleaks` as both a pre-commit hook and a GitHub Actions job in a sample repo. Verify the pre-commit hook blocks a commit containing a Stripe-style key, then verify the CI job fails on a key pushed with `git commit --no-verify`. Explain why you need both gates rather than just one.

3. **(Design)** You inherit a service with a single long-lived Postgres superuser password hardcoded across twelve microservices' config files. Design a migration to dynamic, per-service, short-lived database credentials with zero downtime. Address: how services authenticate to the secret store without a chicken-and-egg bootstrap secret, how you cut over without a flag day, how rotation works afterward, and how you'd detect any service still using the old static password during the transition.

## Further reading

- OWASP, [Secrets Management Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Secrets_Management_Cheat_Sheet.html) — the canonical practitioner reference for storage, rotation, and detection
- HashiCorp, [Vault documentation: Database Secrets Engine](https://developer.hashicorp.com/vault/docs/secrets/databases) — how leased, auto-expiring database credentials work in practice
- AWS, [Secrets Manager rotation documentation](https://docs.aws.amazon.com/secretsmanager/latest/userguide/rotating-secrets.html) — automated rotation with dual-secret windows
- GitHub, [About secret scanning and push protection](https://docs.github.com/en/code-security/secret-scanning/about-secret-scanning) — platform-side detection and partner auto-revocation
- [gitleaks](https://github.com/gitleaks/gitleaks) — open-source secret scanner for pre-commit and CI, with documented detection rules
- NIST SP 800-63B, [Digital Identity Guidelines](https://pages.nist.gov/800-63-3/sp800-63b.html) — authoritative guidance on credential lifecycle and secret handling

> **Security note:** The patterns here protect secrets at rest and in transit, but the next frontier is the *bootstrap* problem one layer deeper than step two solved it — every short-lived credential ultimately chains back to some root of trust, and that root is the highest-value target. Modern infrastructure pushes this anchor down to hardware: workload identity backed by a TPM or a cloud instance identity document, attested at boot, so that the first credential a workload presents is unforgeable rather than a shared bootstrap token sitting in a config. As you adopt dynamic secrets, audit the very first link in the chain — a leaf credential expiring in 15 minutes is no safer than the static token used to mint it. Trace the trust all the way to its root and make sure the root is hardware-anchored, not another secret in a file.
