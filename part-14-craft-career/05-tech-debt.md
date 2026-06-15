# Tech debt as a budget

## Why this matters

It's a Tuesday afternoon and a one-line copy change — swap "Sign up" to "Get started" on the marketing site — has been sitting in review for two days. The change itself took thirty seconds. The problem is that the button text lives in a JSON blob that's read by a templating layer that was bolted onto a legacy PHP renderer that nobody on the current team wrote, and touching it requires a full staging deploy because there are no tests around it and the last person who shipped a "small" change there took down checkout for forty minutes. So the thirty-second change costs two engineers half a day of careful manual verification.

Multiply that by every change that flows through that subsystem, for the next two years, and you start to see the shape of the cost. The interest on this debt is not a metaphor for a feeling. It is real engineering hours, spent every week, that buy you nothing — no new feature, no fixed bug, just the tax of working inside a structure that resists change. And like financial interest, it compounds: the slower it is to change, the less anyone changes it, the more it rots, the slower it gets.

The engineers who can't articulate this lose the argument every time. They go to their PM and say "we need to refactor the checkout templating," the PM hears "engineers want to spend two weeks polishing something customers can't see," and the work never gets prioritized. The engineers who win the argument come with a number: "this subsystem costs us roughly a day of engineering per week in slowdown and risk; a two-week investment pays for itself in ten weeks and removes our highest-frequency incident." That sentence is the entire skill of this chapter. Debt you can name, measure, and price is debt you can get permission to pay down.

## Mental model

The financial metaphor is the one Ward Cunningham coined, and it's worth using precisely rather than loosely. You take on debt to ship sooner. You pay *interest* — extra effort on every future change — for as long as the debt exists. You can pay down the *principal* (fix the underlying structure) to stop the interest, or you can keep servicing the interest forever. The decision to take on debt is sometimes the right one: shipping a quick implementation to validate a market beats a perfect implementation of a feature nobody wants. Cunningham's original point was that debt is acceptable *as long as you pay it back* and you understand what you borrowed.

The crucial refinement, due to Martin Fowler, is that not all debt is the same and not all of it is bad. His quadrant separates *prudent vs. reckless* debt from *deliberate vs. inadvertent* debt:

```mermaid
quadrantChart
    title Fowler's tech debt quadrant
    x-axis Inadvertent --> Deliberate
    y-axis Prudent --> Reckless
    quadrant-1 Deliberate and reckless (no time for design)
    quadrant-2 Inadvertent and reckless (what is layering)
    quadrant-3 Inadvertent and prudent (now we know better)
    quadrant-4 Deliberate and prudent (ship now, refactor next)
```

The two diagonals tell different stories. Deliberate-prudent debt ("we'll ship the hardcoded config now and generalize it once we have three customers") is a legitimate engineering tool. Inadvertent-prudent debt ("now that the feature exists, we see the abstraction we should have used") is the unavoidable cost of learning, and it's where most real debt actually comes from — not from laziness, but from understanding the problem better after building it. The reckless row is where you lose money: shortcuts taken without understanding the cost, or worse, without knowing a cost was incurred at all.

The budget framing follows directly. A team has a finite capacity per quarter. Some of it goes to features (new principal you're investing in growth), some to interest payments (the slowdown tax on existing debt), and some can go to paying down principal (refactoring to reduce future interest). When interest payments eat too much of the budget, velocity collapses and no amount of "working harder" fixes it — you're servicing debt. The job of an engineer making the business case is to show that a one-time principal payment lowers the recurring interest, freeing budget for features. That is an investment argument, and investment arguments are ones non-engineers already know how to evaluate.

## In practice

### Name the debt where it lives

Debt that lives only in someone's head is invisible to planning and dies when they leave. Write it down at the site of the problem and in your tracker. A lightweight in-code marker keeps the context next to the code:

```typescript
// TECH-DEBT(JIRA-4821): checkout templating reads button copy from
// untyped JSON and has no test coverage. Every copy change requires a
// full staging deploy + manual verification (~0.5 day/change, ~2 changes/wk).
// Principal: extract a typed config + snapshot tests. Est. 1.5 wk.
// Interest: ~1 eng-day/week + recurring P2 incident risk.
export function renderCheckoutCopy(raw: unknown): string {
  // ...
}
```

The marker is structured on purpose: a ticket reference, what the debt *is*, the interest (recurring cost), the principal (cost to fix), and an estimate. A `grep -r "TECH-DEBT"` across the repo becomes an instant inventory. Don't use bare `// TODO` for this; TODOs are wishes, and they accumulate into noise that everyone learns to ignore.

### A debt register that a PM can read

Maintain a single register — a tracker epic, a wiki table, whatever your team reads — with one row per item. The columns that matter:

| ID | Debt | Interest (recurring cost) | Principal (fix cost) | Blast radius | Payback |
|---|---|---|---|---|---|
| 4821 | Checkout copy in untyped JSON, no tests | ~1 eng-day/wk + P2 risk | 1.5 wk | Revenue path | ~10 wk |
| 5102 | Auth service has no integration tests | Slows every auth change, fear-driven | 1 wk | All logins | High |
| 4990 | Duplicated date logic in 4 services | Bugs fixed 4x, drift | 3 days | Reporting only | Medium |
| 5340 | Node 16 EOL on payments worker | Security exposure, no patches | 4 days | Compliance | Urgent |

The "payback" column is the lever for prioritization. Where you can estimate it in weeks ("this fix pays for itself in 10 weeks"), do — that's the most persuasive form. Where you can't quantify cleanly (security, compliance, attrition risk), state the qualitative consequence honestly rather than inventing a number.

### Classify before you prioritize

Not every item competes on the same axis. A useful first cut sorts debt into three buckets, because they get funded differently:

1. **Maintenance debt** — outdated dependencies, EOL runtimes, deprecated APIs. This is non-negotiable and best handled as a steady drip (an "evergreen" budget) rather than a big-bang. Item 5340 is here. Frame it as risk, not improvement; a Node EOL is a security and compliance issue, which is a language executives fund readily.
2. **Friction debt** — the stuff that slows daily work: missing tests, tangled modules, painful deploys. Items 4821 and 5102. This is where the interest metaphor is strongest and where payback math is most convincing.
3. **Architectural debt** — a fundamental design that no longer fits, e.g. a monolith that should be split, a data model that can't represent the business anymore. These are the expensive, high-blast-radius items that need to go through the RFC process (see the chapter on technical RFCs in this part), not a sprint ticket.

### Prioritize with a cheap scoring model

You don't need a heavy framework. Borrow the WSJF idea from SAFe — cost of delay divided by job size — without the ceremony:

```text
priority_score = (interest_per_week * weeks_until_paid_down + risk_weight) / principal_cost
```

Item 4821: interest ~1 day/week, principal 1.5 weeks, low extra risk. High score because the divisor is small and the interest is steady. Item 5102 (auth tests) has lower direct interest but a high `risk_weight` because the blast radius is "all logins." The point of the formula isn't false precision — it's forcing every item onto the same two axes (recurring pain vs. cost to fix) so the conversation is about comparable things.

### Pay it down without asking permission for everything

The most sustainable model is a standing allocation rather than a special project. A common and defensible split is something like 70% features / 20% debt and reliability / 10% exploration, adjusted to your context. The 20% is yours to spend on the top of the register without a separate business case for each item — that's the whole point of having budgeted it. Reserve the explicit business case for the large architectural items that exceed the standing allocation.

The "boy scout rule" — leave the code a little better than you found it — works for small friction debt that's near code you're already touching. It does *not* work for architectural debt, and pretending it does is how teams convince themselves they're addressing debt while the big problems metastasize. Opportunistic cleanup and budgeted paydown are complements, not substitutes.

### Make the business case in their language

When you do need explicit sign-off, translate. Engineers say "the auth service has no tests." A PM or director hears nothing actionable. Reframe in terms of the three things non-engineers actually optimize: speed, risk, and cost.

> "Every change to login is slow and scary because we can't verify it automatically, so we ship auth changes about half as fast as everything else and we've had two login outages this year. A one-week investment in an integration test suite would let us ship auth changes at normal speed and cut that outage risk. Given we touch auth roughly weekly, it pays back inside a quarter."

Speed (ship at normal velocity), risk (cut outages), payback (one quarter). No jargon, a number where one is honest, and a qualitative risk statement where it isn't. That paragraph gets funded. "We need to refactor auth" does not.

> **Connect the dots:** The estimates in your debt register inherit every weakness from the chapter on estimation in this part. A "1.5 week" principal is a forecast with a confidence interval, not a fact — present it as a range, and use reference classes (how long did the *last* similar refactor actually take?) rather than gut feel.

## Pitfalls and anti-patterns

**The Big Rewrite.** When interest gets unbearable, the tempting move is to throw it all away and rebuild clean. You recognize it by the phrase "it'll be faster to start over." It almost never is. The rewrite has to hit a moving target (the old system keeps shipping features), it discards hard-won bug fixes encoded in the old code's ugliness, and it delivers no value until it's complete — often years. The fix: strangler-fig migration. Wrap the old system, route new functionality through the new implementation incrementally, and shrink the legacy surface over time so every step ships value and the old system is always still running.

**Debt theater.** A team declares "tech debt sprint," spends two weeks bumping dependency versions and renaming variables, feels productive, and the actual high-interest items — the untested revenue path, the unsplittable monolith — are untouched because they're hard. You recognize it when the debt register doesn't change ranks after the sprint. The fix: pay down from the *top* of a prioritized register, and measure interest before and after (deploy frequency to that subsystem, incident count, change lead time). If the metrics don't move, you cleaned the wrong thing.

**The TODO graveyard.** Thousands of `// TODO` and `// FIXME` comments accumulate with no owner, no ticket, no cost estimate. Everyone has learned to scroll past them, so they're worse than nothing — they're a signal that markers in this codebase are meaningless. The fix: distinguish a wish (`TODO`) from tracked debt (`TECH-DEBT(TICKET)`), require the latter to link a real ticket with an interest/principal estimate, and periodically sweep bare TODOs into either a ticket or the trash.

**Debt as a moral failing.** Treating all debt as evidence that someone was sloppy poisons the conversation. Most real debt is inadvertent-prudent — the team learned something after building, exactly as Fowler describes — or deliberate-prudent debt that was the right call at the time. When debt is framed as blame, engineers stop logging it (who wants to advertise their own failures?) and it goes back underground. The fix: log debt neutrally, as a normal byproduct of shipping under uncertainty, and separate "should we pay this down" from "who caused it."

**Gold-plating in disguise.** Sometimes "tech debt" is the label engineers attach to "code that isn't written the way I prefer." Refactoring code that is stable, rarely touched, and causes no measurable slowdown is not paying down debt — it's spending budget for aesthetic comfort. You recognize it when the "debt" has near-zero interest: nobody touches that file and nothing breaks. The fix: require an interest column. Debt with no recurring cost is not a priority, no matter how ugly it is. Ugly-but-stable code at the edge of the system can be left alone.

## Production checklist

- [ ] A single debt register exists, is owned, and is reviewed at least once per planning cycle
- [ ] Every register item has both an *interest* (recurring cost) and a *principal* (cost to fix) field
- [ ] Interest is quantified in eng-time or incident risk wherever honestly possible; qualitative where not
- [ ] In-code markers use a structured `TECH-DEBT(TICKET-ID)` form that links to the register, not bare `TODO`
- [ ] Debt is classified as maintenance / friction / architectural, because each is funded differently
- [ ] A standing capacity allocation (e.g. ~20%) is reserved for debt + reliability, separate from feature work
- [ ] Maintenance debt (EOL runtimes, deprecated deps) is handled as a steady drip and framed as risk/compliance
- [ ] Architectural debt goes through an RFC, not a sprint ticket
- [ ] Each major paydown defines a before/after metric (deploy frequency, lead time, incident count) to confirm interest actually dropped
- [ ] The business case for any large item is written in speed/risk/cost terms with a payback period where one is defensible

## Exercises

1. **(Comprehension)** Take the four-quadrant model and classify three pieces of debt from a codebase you know: place each as deliberate/inadvertent and prudent/reckless. For each, state in one sentence what the *interest* is — the specific recurring cost you pay because the debt exists. If you can't name the interest, ask whether it's really debt or just code you dislike.

2. **(Applied)** Build a debt register for a real subsystem. List 5–8 items with columns for interest (recurring cost), principal (fix cost), and blast radius. Estimate a payback period for at least two of them. Then rank the list using the scoring formula from this chapter and write the one-paragraph business case, in speed/risk/cost language, for the top item — as if you were pitching it to a non-engineering manager.

3. **(Open-ended design)** Your team's velocity has dropped noticeably over a year with no change in headcount, and leadership believes the team has "gotten lazy." You believe it's interest on accumulated debt. Design a six-month plan to (a) make the debt and its cost visible to leadership, (b) negotiate a sustainable paydown allocation, and (c) prove the paydown is working with metrics. Specify what you'd measure, how you'd avoid debt theater, and how you'd decide between strangler-fig migration and incremental in-place fixes for your largest architectural item.

## Further reading

- Ward Cunningham, ["The WyCash Portfolio Management System"](https://c2.com/doc/oopsla92.html) (OOPSLA 1992) and his later [video on the debt metaphor](https://www.youtube.com/watch?v=pqeJFYwnkjE) — the origin of the term, and a correction of how it's usually misused
- Martin Fowler, ["Technical Debt Quadrant"](https://martinfowler.com/bliki/TechnicalDebtQuadrant.html) — the deliberate/inadvertent × prudent/reckless framing used in this chapter
- Martin Fowler, ["Is High Quality Software Worth the Cost?"](https://martinfowler.com/articles/is-quality-worth-cost.html) — the argument that internal quality pays for itself, in language usable with non-engineers
- Philippe Kruchten, Robert Nord, Ipek Ozkaya, *Managing Technical Debt* (Addison-Wesley, 2019) — the most rigorous book-length treatment, including a principal/interest measurement model
- Martin Fowler, ["StranglerFigApplication"](https://martinfowler.com/bliki/StranglerFigApplication.html) — the incremental alternative to the Big Rewrite
- Nicole Forsgren, Jez Humble, Gene Kim, *Accelerate* (IT Revolution, 2018) — the DORA metrics (deploy frequency, lead time, change-fail rate, MTTR) you can use to measure whether paydown actually reduced interest
