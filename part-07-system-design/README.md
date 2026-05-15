# Part 7 — System Design & Distributed Systems

Scaling patterns, load balancing, service mesh, microservices, event-driven architecture, event sourcing, CQRS, saga, consensus, idempotency, and the classic design exercises.

## Why this part exists

System design interviews test whether you can reason about tradeoffs you've never seen at scales you've never operated. The trick is that there are only ~20 reusable patterns. This part teaches those patterns, with the ugly edges intact.

## Chapters in this Part

1. **Scaling vertically, then horizontally** — When each is the right move and what each demands of the application.
2. **Load balancing** — L4 vs. L7, sticky sessions, health checks, traffic shifting, and the failure modes of each.
3. **Microservices in practice** — Service boundaries, contracts, deployment isolation, and the operational cost rarely budgeted up front.
4. **Event-driven architecture** — Events vs. commands, choreography vs. orchestration, schema evolution, and the parts that age badly.
5. **Event sourcing and CQRS** — When the audit trail justifies the complexity and how to recover from broken projections.
6. **The saga pattern** — Distributed transactions without two-phase commit, and the compensation logic you have to write yourself.
7. **Consensus and replication** — Raft, Paxos, leader election: enough understanding to debug them when they break.
8. **Idempotency and the exactly-once illusion** — Why "exactly once" is a lie at the network layer, and the application patterns that survive it.
