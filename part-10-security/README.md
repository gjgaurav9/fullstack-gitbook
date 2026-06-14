# Part 10 — Security

OWASP Top 10, authentication and authorization models, cryptography, TLS, secrets management, supply chain security, container security, threat modeling, and compliance touchpoints.

## Why this part exists

Every application engineer is a security engineer whether they want the title or not. The attacks aren't theoretical, the threat actors are automated, and the cost of one credential leak can erase a year of feature work. This part is the working knowledge a senior engineer should already have.

## Chapters in this Part

1. **[The OWASP Top 10, demonstrated](01-owasp-top-10.md)** — Each class of attack with a working example, the detection signal, and the fix.
2. **[AuthN/AuthZ models that scale](02-authn-authz-models.md)** — RBAC, ABAC, ReBAC, policy engines, and choosing between them.
3. **[Applied cryptography](03-applied-cryptography.md)** — Hashing, symmetric and asymmetric encryption, signatures, KDFs, and the algorithms you should never roll yourself.
4. **[TLS, certificates, and the trust chain](04-tls-certificates.md)** — Handshakes, ALPN, mTLS, cert lifecycle, and the failure modes.
5. **[Secrets management](05-secrets-management.md)** — Vaults, KMS, rotation, environment hygiene, and the patterns that prevent credential leaks.
6. **[Supply chain and dependency security](06-supply-chain-security.md)** — SBOMs, signed artifacts, lockfile auditing, typosquatting, and the parts CI should catch.
7. **[Threat modeling and compliance touchpoints](07-threat-modeling-compliance.md)** — STRIDE, data classification, and the SOC 2 / GDPR / HIPAA basics every engineer should know.
