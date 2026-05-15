# Contributing

Thanks for considering a contribution. This handbook aims for a single coherent voice across 80+ chapters, so contributions are filtered tightly for style and depth. Read the rules below before opening a PR.

## What we accept

- **Chapter drafts** for a planned chapter in the [outline](SUMMARY.md), following the template below.
- **Corrections** to factual errors, broken code, broken diagrams, dead links.
- **New exercises** or improved exercises for an existing chapter.
- **Cross-link suggestions** ("Connect the dots" callouts between chapters).

## What we don't accept

- New chapters outside the outline without first opening an issue to discuss.
- Stylistic rewrites that change the voice.
- "Cleanup" PRs that touch many files for low-signal changes.

## The voice rules

This is non-negotiable. The Git internals chapter at `part-03-version-control/01-git-internals.md` is the style anchor. Re-read it before drafting anything.

1. **Lead with a real engineering scenario.** Not a generic intro. A specific moment where this knowledge matters.
2. **Concrete before abstract.** Show code first. Theorize second.
3. **Runnable code only.** TypeScript for app code, Python for data/AI, Go for systems, SQL for databases. Language tag on every block.
4. **Mermaid diagrams** for any structural concept.
5. **Anti-patterns before correct patterns.** Showing the wrong way teaches better.
6. **Cite primary sources** — papers, RFCs, official docs — over blog posts.
7. **Opinionated.** When the field disagrees, name the tradeoff and pick a side.

### Forbidden phrases

These read as AI-written and are auto-rejected:

`delve`, `tapestry`, `in the realm of`, `it's worth noting`, `in conclusion`, `in today's fast-paced world`, `navigate` used as a metaphor, and em-dash-heavy prose.

## The chapter template

Every chapter has these seven sections, in this order:

1. **Why this matters** — 2–3 paragraphs, real scenario.
2. **Mental model** — minimum theory, at least one Mermaid diagram.
3. **In practice** — working code, real configs, real commands.
4. **Pitfalls and anti-patterns** — 3–5 named failure modes with detection and fix.
5. **Production checklist** — copy-pasteable.
6. **Exercises** — three: comprehension, applied, design.
7. **Further reading** — 3–7 curated primary sources.

Where relevant, end with:

- `> Connect the dots` callout linking concepts across parts.
- `> Security note` for any backend or DevOps chapter.

**Length:** 2,000–4,000 words. Depth over breadth.

## Workflow

1. Fork the repo.
2. Branch from `main`: `feat/part-XX-chapter-slug`.
3. Draft your chapter.
4. Run the quality checks below before pushing.
5. Open a PR with the title `docs(part-XX): add chapter "Title"`.

## Quality checks (run before pushing)

- [ ] Every code block has a language tag and is syntactically valid.
- [ ] Every Mermaid diagram renders cleanly at [mermaid.live](https://mermaid.live).
- [ ] No forbidden phrases.
- [ ] Chapter is 2,000–4,000 words.
- [ ] All seven template sections present.
- [ ] Internal links use relative paths and resolve.
- [ ] `SUMMARY.md` updated.

## Local preview

```bash
npm install -g honkit
honkit serve
```

Open `http://localhost:4000`.

## License

By contributing you agree your work is licensed under [MIT](LICENSE).
