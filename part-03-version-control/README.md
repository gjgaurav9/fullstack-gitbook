# Part 3 — Version Control & Collaboration

Git, deeper than you've seen it. Branching strategies that actually scale. PR workflows that don't grind teams to a halt. Monorepos, conventional commits, semver.

## Why this part exists

Every engineer uses Git daily. Most use 12 commands and treat the rest as magic. When the magic fails — a botched rebase, a force-push that erased a teammate's commits, a merge conflict in a generated lockfile — knowing what's under the hood is the difference between recovery in two minutes and three hours.

## Chapters in this Part

1. **[Git internals](01-git-internals.md)** — How Git actually stores history as a content-addressable tree, and why every advanced operation is just a tree walk. *Style anchor for the rest of the book.*
2. **[Branching strategies that scale](02-branching-strategies.md)** — Trunk-based development, GitFlow, release branches, and which fits which team size and release cadence.
3. **[The pull-request workflow](03-pull-request-workflow.md)** — Writing reviewable PRs, code-review etiquette, merge vs. rebase vs. squash, draft PRs, stacked PRs.
4. **[Monorepos vs. polyrepos](04-monorepos-vs-polyrepos.md)** — Tradeoffs in tooling, ownership, CI cost, and the criteria that should drive the choice.
5. **[Conventional commits and semantic versioning](05-conventional-commits-semver.md)** — The contract that lets your changelog and release notes write themselves.
6. **Recovering from Git disasters** — Reflog, dangling commits, accidental force-push, garbage collection: the safety net no one teaches.
