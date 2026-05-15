# Part 12 — AI Engineering

LLM fundamentals, prompt engineering, RAG, vector databases, agents, Model Context Protocol (MCP), evaluations, guardrails, the cost/latency/quality triangle, and AI-assisted development.

## Why this part exists

By 2026, "AI engineer" is no longer a specialist title — it's the table stakes for senior full-stack work. This part treats LLM applications the way the rest of the book treats web applications: a real engineering discipline with patterns, tradeoffs, and production failure modes worth knowing before you ship.

## Chapters in this Part

1. **LLM fundamentals for application engineers** — Tokens, context windows, sampling, temperature, and what the model can and can't reliably do.
2. **Prompt engineering as a real skill** — Structure, examples, chain-of-thought, system prompts, and eval-driven iteration.
3. **RAG done well** — Chunking, embedding, retrieval strategies, reranking, and the failure modes most "just bolt on a vector store" tutorials skip.
4. **Vector databases** — pgvector, Pinecone, Weaviate, Qdrant: when each fits and how to migrate between them.
5. **Agents and tool use** — Function calling, multi-step reasoning, when an agent earns its keep and when it's overkill.
6. **Model Context Protocol (MCP)** — The 2026 standard for connecting LLMs to tools and data, and how to build and consume MCP servers.
7. **Evals and guardrails** — Building eval suites, output validation, safety filters, and treating LLMs like any other unreliable dependency.
8. **The cost/latency/quality triangle** — Choosing models, batching, caching, and the FinOps of AI in production.
