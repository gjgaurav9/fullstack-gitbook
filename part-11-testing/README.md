# Part 11 — Testing & Quality

The test pyramid, TDD, BDD, property-based testing, mutation testing, contract testing, performance testing, and static analysis.

## Why this part exists

Most engineers write tests that pass when the code passes and fail when the code fails — a tautology. This part teaches tests that catch real bugs: tests that find the failure mode you didn't think of, tests that prevent the next regression, tests that hold contracts between services. Coverage is the wrong metric. This part covers the right ones.

## Chapters in this Part

1. **[The test pyramid, revisited](01-test-pyramid.md)** — Unit, integration, end-to-end: what each catches, what each costs, and the right ratio for your stack.
2. **[TDD and BDD](02-tdd-bdd.md)** — The discipline, the payoff, and where each breaks down.
3. **[Property-based and mutation testing](03-property-mutation.md)** — Finding the bugs your example-based tests miss.
4. **[Contract testing](04-contract-testing.md)** — Pact, consumer-driven contracts, schema registries, and the failure modes of integration tests.
5. **[Performance and load testing](05-performance-testing.md)** — k6, locust, percentiles, baselines, and regression detection in CI.
6. **[Static analysis, linters, formatters](06-static-analysis.md)** — Pre-merge quality gates that pay for themselves in a week.
