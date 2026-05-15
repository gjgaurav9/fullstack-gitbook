# Part 5 — Backend Engineering

API design (REST, GraphQL, gRPC), frameworks, auth (OAuth, OIDC, passkeys), architectural patterns (hexagonal, DDD), messaging (Kafka, SQS), caching, background jobs, real-time, rate limiting.

## Why this part exists

A working backend looks simple from the outside: requests in, responses out. Inside, every decision — sync vs. async, REST vs. RPC, where to put the cache, what gets a queue — has tradeoffs that compound at scale. This part is the decision frame, not the framework tour.

## Chapters in this Part

1. **API design** — REST done right, GraphQL where it pays off, gRPC where speed and contracts matter, and choosing between them.
2. **Authentication and authorization** — OAuth 2.1, OIDC, passkeys, JWTs, session cookies, and the failure modes of each.
3. **Architectural patterns** — Layered, hexagonal, ports-and-adapters, DDD: which structure actually helps which kind of application.
4. **Messaging and queues** — Kafka, SQS, RabbitMQ, NATS: when each is the right call and how to use them without data loss.
5. **Caching layers** — In-memory, Redis, CDN, application-level: TTLs, invalidation, and the thundering herd.
6. **Background jobs and schedulers** — Queues, cron, workers, retries, dead-letter queues, and idempotency for replays.
7. **Real-time and streaming** — WebSockets, Server-Sent Events, long polling, pub/sub at scale.
8. **Rate limiting and quotas** — Token bucket, sliding window, distributed counters, and protecting upstream systems from your traffic.
