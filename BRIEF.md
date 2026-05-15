# Project Brief — The Modern Full-Stack Developer Handbook

> This is the master brief for Claude Code. Read it fully before doing anything
> else. Re-read it whenever context resets. Every decision should trace back
> to something in this document.

---

## 1. The Vision

I am **Gaurav Jain** — Senior Software Engineer, 12+ years full-stack,
positioning myself as an "AI-First Engineer" targeting 100% remote roles at
companies like Shopify, Cloudflare, DigitalOcean, DocuSign, Confluent,
Automattic, Crossover, and Mercor.

This GitBook is a **portfolio-grade artifact** that does three jobs at once:

1. **Proof of depth.** A comprehensive, opinionated handbook that demonstrates
   I can teach what I know — across CS foundations, full-stack, DevOps, system
   design, and AI engineering.
2. **A living reference for me.** My own second brain for interviews,
   coaching, and ongoing skill grooming.
3. **A community asset.** Public, open-source, with a contribution path — so
   it compounds in value over time.

The book is titled:
**"The Modern Full-Stack Developer: An Essential Skills Handbook (2026 Edition)"**

It is opinionated, technically precise, and written in a single coherent
voice. Not a tutorial site. Not an encyclopedia. A handbook that a
0–3 year engineer can read end-to-end and emerge competent, and that a senior
engineer can keep on the shelf as a reference.

---

## 2. The Audience

- **Primary:** Engineers with 0–3 years of experience who want a single source
  of truth to fill gaps and grow toward senior.
- **Secondary:** Self-taught developers, bootcamp graduates, CS students
  bridging theory to practice, senior engineers using it as a reference.

Write for someone smart who has gaps, not someone slow who needs hand-holding.

---

## 3. The Voice (Non-Negotiable)

Conversational but technically precise. The reference points are John
Ousterhout (precise), Martin Kleppmann (rigorous), and Julia Evans
(clear and warm). Not generic tutorial site.

**Rules:**
- Lead every chapter with a "Why this matters" scenario — a real engineering
  moment where this knowledge is the difference between shipping and not.
- Concrete examples before abstract definitions. Show code first, theorize
  second.
- Code samples must be runnable. Default to TypeScript for app code, Python
  for data/AI, Go for systems, SQL for databases.
- Diagrams as Mermaid blocks whenever a concept is structural.
- Show the wrong way before the right way — anti-patterns teach better.
- Cite primary sources (papers, RFCs, official docs) over blog posts.
- Opinionated takes encouraged. When the field disagrees, present tradeoffs
  then state what you'd pick and why.
- **Forbidden words/phrases:** "delve", "tapestry", "in the realm of",
  "it's worth noting", "in conclusion", "in today's fast-paced world",
  "navigate" (as a metaphor), em-dash-heavy prose that feels AI-written.
- No padding. If a section is one paragraph, leave it one paragraph.

**Style anchor:** The Git chapter at `/part-03-version-control/01-git-internals.md`
is the voice. Every chapter must feel like the same author wrote it. Re-read
the Git chapter before drafting any new chapter.

---

## 4. The Structure

15 Parts. Each Part has a README.md (overview + learning outcomes) and
multiple chapter files. Final structure:

```
fullstack-handbook/
├── README.md                          # Book intro
├── SUMMARY.md                         # GitBook ToC
├── .gitbook.yaml                      # GitBook config
├── CONTRIBUTING.md                    # How to contribute
├── LICENSE                            # MIT
├── BRIEF.md                           # This file
├── part-01-cs-foundations/
├── part-02-programming/
├── part-03-version-control/
│   └── 01-git-internals.md            # SEED CHAPTER (style anchor)
├── part-04-frontend/
├── part-05-backend/
├── part-06-databases/
├── part-07-system-design/
├── part-08-devops-cloud/
├── part-09-observability/
├── part-10-security/
├── part-11-testing/
├── part-12-ai-engineering/
├── part-13-architecture/
├── part-14-craft-career/
└── part-15-capstones/
```

### Part-by-Part Skills Inventory

**Part 1 — CS Foundations:** data structures, algorithms, complexity, OS
fundamentals, networking (TCP/IP, HTTP, DNS, TLS), concurrency primitives,
discrete math essentials.

**Part 2 — Programming Foundations:** language paradigms, type systems,
memory model, error handling, async patterns. At least one static and one
dynamic language covered with depth.

**Part 3 — Version Control & Collaboration:** Git internals (SEED),
branching strategies, PR workflow, monorepos, conventional commits,
semver. *Git seed chapter already written.*

**Part 4 — Frontend Engineering:** HTML/CSS/JS modern features, React
(hooks, RSC), Next.js, state management, Tailwind, performance (Core Web
Vitals), a11y, browser APIs, build tooling, testing, PWAs.

**Part 5 — Backend Engineering:** API design (REST, GraphQL, gRPC),
frameworks, auth (OAuth, OIDC, passkeys), architectural patterns
(hexagonal, DDD), messaging (Kafka, SQS), caching, background jobs,
real-time, rate limiting.

**Part 6 — Databases & Data:** PostgreSQL deep-dive, MongoDB, Redis,
search (Elasticsearch), time-series, graph, vector DBs, ACID/BASE/CAP,
scaling, modeling, ORMs.

**Part 7 — System Design & Distributed Systems:** scaling patterns, load
balancing, service mesh, microservices, event-driven, event sourcing,
CQRS, saga, consensus, idempotency, classic design exercises.

**Part 8 — DevOps, Infrastructure & Cloud:** Linux fundamentals, Docker,
Kubernetes, CI/CD, IaC (Terraform), AWS-primary cloud coverage,
serverless, FinOps.

**Part 9 — Observability & Reliability:** logs/metrics/traces,
OpenTelemetry, SLOs, incident management, on-call, chaos engineering.

**Part 10 — Security:** OWASP Top 10, AuthN/AuthZ models, cryptography,
TLS, secrets management, supply chain, container security, threat
modeling, compliance touchpoints.

**Part 11 — Testing & Quality:** test pyramid, TDD, BDD, property-based,
mutation testing, contract testing, performance testing, static analysis.

**Part 12 — AI Engineering (2026 essential):** LLM fundamentals, prompt
engineering, RAG, vector DBs, agents, MCP (Model Context Protocol), evals,
guardrails, cost/latency/quality tradeoffs, AI-assisted development.

**Part 13 — Architecture & Design:** SOLID, design patterns (where they
apply), DDD, ADRs, API versioning, compatibility, C4 diagrams.

**Part 14 — Engineering Craft & Career:** reading code, code review,
writing docs/RFCs, estimation, tech debt, mentoring, async work, career
ladders.

**Part 15 — Capstone Projects:** real-time collaborative editor,
multi-tenant SaaS with billing, AI semantic search, distributed URL
shortener, event-sourced ledger, self-hosted PaaS on Kubernetes.

---

## 5. Chapter Template (Mandatory)

Every chapter follows this exact structure:

```markdown
# Chapter Title

## Why this matters
[2–3 paragraph scenario showing the cost of not knowing this]

## Mental model
[Minimum theory, with at least one Mermaid diagram]

## In practice
[Working code, real configs, real commands. Multiple sub-sections as needed.]

## Pitfalls and anti-patterns
[3–5 concrete failure modes with how to recognize and fix them]

## Production checklist
[Bulleted, copy-pasteable list of what "done well" looks like]

## Exercises
[3 exercises: one comprehension, one applied, one open-ended design]

## Further reading
[3–7 curated links: papers, books, official docs, seminal blog posts]
```

Plus, where relevant:
- **"Connect the dots"** callouts linking concepts across parts (block
  quote with `>`)
- **"Security note"** at the end of every backend/DevOps chapter (block
  quote with `>`)

**Length:** 2,000–4,000 words per chapter. Depth over breadth within a
chapter; surface area comes from chapter count.

---

## 6. The Seed Chapter (Style Anchor)

`/part-03-version-control/01-git-internals.md` is already written. It is
the canonical example of voice, structure, and density. **Before writing
any new chapter, re-read it.** Every new chapter should feel like it came
from the same author.

Key qualities to match:
- Opens with a specific Tuesday-afternoon scenario, not a generic intro
- Mental model section uses a Mermaid diagram and walks through concepts
- "In practice" has runnable code with real output shown
- Pitfalls are named failure modes, not vague warnings
- Production checklist is copy-pasteable
- Exercises escalate: comprehension → applied → design
- "Connect the dots" and "Security note" callouts at the end

---

## 7. The Build Process (Passes)

Work in **passes**, one at a time. I will tell you which pass to run.

**Pass 1 — Outline.** Generate `SUMMARY.md`, `README.md`, and per-Part
`README.md` files with chapter titles and one-line abstracts. No chapter
content yet. Stop and wait for my review.

**Pass 2 — Chapter drafts.** I will name a Part. Generate full chapters
for that Part, one file per chapter, complete to the template. Match the
Git seed chapter's voice. Commit after each chapter with a Conventional
Commit message.

**Pass 3 — Exercises and capstones.** For each Part, generate a separate
`exercises.md` with solutions in a collapsed `<details>` block.

**Pass 4 — Cross-linking.** Add "Connect the dots" callouts and update
`SUMMARY.md` anchors.

**Pass 5 — Polish.** Tighten prose, verify code samples run, add missing
diagrams.

After each pass, run the checks in Section 9 before declaring it done.

---

## 8. The Deployment Plan

**Hosting:** GitBook hosted, free plan to start. Bi-directional GitHub
Sync — push to `main`, GitBook publishes. Web edits in GitBook commit back
to the repo.

**Sequence:**
1. Initialize the repo locally with the structure in Section 4.
2. Push to GitHub as a public repo: `github.com/gjgaurav9/fullstack-handbook`.
3. Verify the seed chapter renders correctly locally with Honkit first
   (so we know the Markdown is valid before paying GitBook any attention):
```bash
   npm install -g honkit
   honkit serve
```
4. Once the seed chapter renders cleanly, sign up at gitbook.com on the
   free plan, create a Space, configure GitHub Sync pointing at the repo
   and `main` branch. Initial sync direction: **GitHub → GitBook**.
5. Verify the published `*.gitbook.io` URL renders. **Do not generate the
   other 80 chapters until this pipeline is confirmed working end-to-end
   with one chapter.**
6. Apply for the GitBook open-source discount once `CONTRIBUTING.md`,
   `LICENSE`, and the seed chapter are merged.
7. Custom domain (`handbook.gauravjain.dev`) deferred until ~15 chapters
   are live and I decide whether to stay on GitBook Premium ($65/mo) or
   migrate to self-hosted Docusaurus on Cloudflare Pages.

**Required files for deployment:**

`.gitbook.yaml`:
```yaml
root: ./
structure:
  readme: README.md
  summary: SUMMARY.md
```

`LICENSE`: MIT.

`CONTRIBUTING.md`: explain the chapter template, voice rules, how to
propose new chapters, the PR workflow.

---

## 9. Quality Checks (Run Before Every Commit)

Before pushing any chapter or batch of chapters, verify:

- [ ] Every code block has a language tag and is syntactically valid
      (compile/run it where feasible — use Node/Python REPL or
      `npx tsx` for TS snippets)
- [ ] Every Mermaid diagram renders without syntax errors (validate at
      mermaid.live if uncertain)
- [ ] No forbidden words/phrases from Section 3
- [ ] Chapter is 2,000–4,000 words
- [ ] All seven template sections present
- [ ] Internal links use relative paths and resolve
- [ ] `SUMMARY.md` includes the new chapter
- [ ] Conventional Commit message: `docs(part-XX): add chapter "..."`

---

## 10. Working Style Rules

- **Plan before generating.** For any pass that touches multiple files,
  show me the file list and a one-sentence plan per file. Wait for ack.
- **Commit early and often.** One chapter per commit. Conventional
  Commits. Push after every batch.
- **No auto-pushing to `main` for new content.** Work on a branch like
  `pass-2/part-04-frontend`, open a PR, let me review, then merge.
- **No silent additions.** Don't add chapters, sections, or files outside
  the inventory in Section 4 without flagging first.
- **Run the quality checks (Section 9) before each commit, not after.**
- **If a fact needs a source and you don't have one, omit it or flag
  `[VERIFY]` in the text.** Don't invent benchmarks, quotes, or stats.
- **When something is genuinely ambiguous, ask one focused question
  rather than guessing.**

---

## 11. Immediate Next Steps

Do these in order. Do not skip.

1. **Confirm you've read this brief and the Git seed chapter at
   `/part-03-version-control/01-git-internals.md`.** Summarize the voice
   and the chapter template back to me in your own words. If anything is
   ambiguous, ask now.

2. **Set up the repo skeleton.** Create the directory structure from
   Section 4. Create `README.md`, `LICENSE` (MIT), `CONTRIBUTING.md`,
   `.gitbook.yaml`, an empty `SUMMARY.md` that references the Git seed
   chapter, and per-Part `README.md` placeholders. Commit as
   `chore: initialize repo structure`.

3. **Verify local rendering.** Install Honkit, run `honkit serve`,
   confirm the Git seed chapter renders correctly at `localhost:4000`.
   Report any rendering issues.

4. **Run Pass 1: Outline.** Generate the full `SUMMARY.md` and all Part
   `README.md` files with chapter titles and one-line abstracts. Commit
   as `docs: complete book outline (pass 1)`. Open a PR. Stop and wait
   for my review.

5. **After I approve Pass 1:** I'll pick the next Part for Pass 2.
   Suggested order: Part 5 (Backend) first since it's closest to my day
   job and easiest to verify voice consistency, then Part 12 (AI
   Engineering) to lock in the differentiated content, then the rest in
   any order I direct.

---

## 12. Long-Term Vision

This book is not a one-shot project. The 12-month arc:

- **Months 1–3:** Ship Parts 1–5 publicly. Use them as interview talking
  points in remote-role applications.
- **Months 4–6:** Ship Parts 6–10. Start linking from my portfolio site
  and LinkedIn. Solicit feedback from peers.
- **Months 7–9:** Ship Parts 11–15. Capstones become standalone repos
  linked from the book.
- **Months 10–12:** Translate top chapters to Hindi for EngineerReads
  YouTube content. Possibly publish a print edition via a
  self-publishing service.

The book should be **the thing my name is associated with** when a
hiring manager Googles "Gaurav Jain AI-first engineer" by end of 2026.

---

## 13. What "Done" Looks Like for the First Milestone

By end of Pass 1 + verified Pass 2 on Part 3 (Version Control, which
already has the seed chapter, so this just means the other 4–5 chapters
in that part), I should have:

- A live GitBook URL renders the seed chapter and the rest of Part 3
  cleanly
- A public GitHub repo with a clean commit history, CONTRIBUTING.md,
  and LICENSE
- A complete book outline that I've reviewed and approved
- A repeatable process that can crank out the remaining 14 Parts

Everything else is iteration on top of this foundation.
