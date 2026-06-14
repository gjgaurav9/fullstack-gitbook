# Part 6 — Databases & Data

PostgreSQL in depth, then MongoDB, Redis, search (Elasticsearch), time-series, graph, and vector databases. ACID, BASE, and CAP without the hand-waving. Modeling, scaling, ORMs.

## Why this part exists

The database is where your application's hardest problems live. Schema decisions outlive every other architectural choice. Index strategy is the difference between 30ms and 30 seconds. Replication semantics decide whether your "atomic" write is actually atomic. This part takes the database seriously.

## Chapters in this Part

1. **[PostgreSQL deeply](01-postgresql-deep-dive.md)** — MVCC, transactions, isolation levels, indexes, query planning, vacuum, and the parts that bite at scale.
2. **[Data modeling that lasts](02-data-modeling.md)** — Normalization, denormalization, soft deletes, audit trails, time as a first-class column.
3. **[ACID, BASE, and CAP without the slogans](03-acid-base-cap.md)** — What each guarantee actually means, and where engineers misread the tradeoffs.
4. **[Redis as more than a cache](04-redis.md)** — Data structures, pub/sub, streams, Lua scripts, persistence modes, and the right use cases for each.
5. **[MongoDB and document stores](05-mongodb-document-stores.md)** — When schemaless helps, when it hurts, and the patterns for both.
6. **[Search engines](06-search-engines.md)** — Elasticsearch and OpenSearch: tokenizers, analyzers, scoring, relevance tuning, and operating a cluster.
7. **[Time-series, graph, and vector databases](07-specialized-databases.md)** — When the workload demands a specialized store and which ones to evaluate.
8. **[ORMs and query builders](08-orms-query-builders.md)** — The leaky abstraction, when to drop to raw SQL, and migration discipline.
