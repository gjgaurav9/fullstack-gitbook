# Part 2 — Programming Foundations

Language paradigms, type systems, memory models, error handling, and async patterns. The mechanics that sit underneath every framework you'll ever touch.

## Why this part exists

Frameworks come and go. The underlying language semantics — how values are stored, how errors propagate, how concurrency actually works — outlast every framework cycle. Understand the layer below the framework and you stop being surprised.

## Chapters in this Part

1. **Paradigms and when to mix them** — Imperative, functional, object-oriented, declarative: the tradeoffs and where each belongs in real codebases.
2. **Type systems, from dynamic to dependent** — How types catch bugs, where they get in the way, and the gradient from Python through TypeScript and Go to Rust.
3. **Memory models in three languages** — Stack vs. heap, ownership, garbage collection, references vs. values, with Go, Python, and JavaScript side-by-side.
4. **Error handling that survives refactors** — Exceptions vs. result types vs. error returns, and which suits which layer of your system.
5. **Async, threads, and the event loop** — Why "concurrent" and "parallel" are different and what each runtime actually does with your code.
6. **Functions, closures, and the call stack** — The mechanics under every higher-order pattern you'll use.
