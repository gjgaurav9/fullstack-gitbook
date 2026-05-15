# Part 9 — Observability & Reliability

Logs, metrics, traces, OpenTelemetry, SLOs, incident management, on-call practice, and chaos engineering.

## Why this part exists

You can't fix what you can't see. The gap between "the system is broken" and "I know exactly which span on which service in which region is broken" is the difference between a five-minute fix and a four-hour incident. This part teaches how to build that visibility before you need it.

## Chapters in this Part

1. **Logs, metrics, traces** — The three pillars and what each is good for, with the failure modes of relying on only one.
2. **OpenTelemetry in practice** — Instrumentation, context propagation, collectors, exporters, and the patterns that survive a vendor swap.
3. **SLOs, SLIs, and error budgets** — Designing measurable reliability targets that drive engineering decisions.
4. **Incident management** — Detection, response, command structure, comms, and postmortems that change behavior.
5. **On-call practice** — Pages worth waking up for, runbooks that actually help, and rotation health.
6. **Chaos engineering and load testing** — Breaking your system on purpose before it breaks for users.
