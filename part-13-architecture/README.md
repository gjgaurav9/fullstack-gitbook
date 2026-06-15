# Part 13 — Architecture & Design

SOLID, design patterns (where they actually apply), domain-driven design, ADRs, API versioning, compatibility, and C4 diagrams.

## Why this part exists

Architecture is the part of the codebase that's hardest to change later. Get it wrong and every feature costs more for the rest of the system's life. Get it right and the team that comes after you keeps shipping. This part is about the decisions that don't show up in any single commit but show up in every commit.

## Chapters in this Part

1. **[SOLID, honestly](01-solid.md)** — Where each principle actually applies, where it's cargo-culted, and the heuristics worth keeping.
2. **[Design patterns that survived](02-design-patterns.md)** — The handful from the Gang of Four that still earn their keep, and the ones that don't.
3. **[Domain-driven design](03-ddd.md)** — Bounded contexts, ubiquitous language, aggregates: the parts that pay off and the parts that don't.
4. **[Architecture Decision Records](04-adrs.md)** — Writing ADRs your future team will thank you for, and reading old ones charitably.
5. **[API versioning and backward compatibility](05-api-versioning.md)** — URL vs. header versioning, additive change, deprecation timelines, and contract testing.
6. **[C4 diagrams and architecture docs](06-c4-diagrams.md)** — Drawing systems so others can read them without asking you for context.
