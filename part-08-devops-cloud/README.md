# Part 8 — DevOps, Infrastructure & Cloud

Linux fundamentals, Docker, Kubernetes, CI/CD, infrastructure-as-code with Terraform, AWS-primary cloud coverage, serverless, and FinOps.

## Why this part exists

"Works on my machine" stopped being acceptable a decade ago. Modern engineers ship the runtime alongside the code, and the runtime is its own discipline: containers, orchestrators, pipelines, infra definitions, cost. This part covers what every backend or platform engineer needs to operate confidently.

## Chapters in this Part

1. **[Linux for application engineers](01-linux.md)** — Processes, signals, filesystems, networking primitives, systemd, and the tools that tell you what's wrong on a live box.
2. **[Docker beyond `docker run`](02-docker.md)** — Layers, multi-stage builds, security context, build cache, BuildKit, and image hygiene.
3. **[Kubernetes for engineers who must use it](03-kubernetes.md)** — Pods, services, deployments, ingress, configmaps, secrets, RBAC, and the parts you'll actually touch.
4. **[CI/CD pipelines that don't rot](04-cicd.md)** — GitHub Actions, build matrices, caching, deploy strategies, and treating CI as code.
5. **[Infrastructure as code with Terraform](05-terraform.md)** — Modules, state, drift, environment separation, and secrets handling.
6. **[AWS, the working subset](06-aws.md)** — IAM, VPC, EC2, S3, RDS, Lambda, API Gateway, CloudWatch: what 90% of applications actually use.
7. **[Serverless without surprises](07-serverless.md)** — Cold starts, concurrency limits, packaging, and the cost shapes that catch teams off guard.
8. **[FinOps](08-finops.md)** — Reading a cloud bill, tagging, budgets, and the cheapest path to "good enough" performance.
