# Skeleton: blog 2 (what an AI task really costs)

> AI-written (exponential-post skill, move 1: from raw material). Material:
> `outline.md` (rounds 2–3 + material moved from post 1) and `draft.md`
> (AI draft, 2026-11-11). Both are material, not the answer.

## Status

Round 8: skeleton holds. Next: Steff writes (after blog 1).

## Skeleton (round 8)

| Part | Content | Budget |
|---|---|---:|
| **Question** | "What is the business case for building the fallback?" Picks up blog 1's cliffhanger: convinced of the supply risk, now the manager asks whether it is worth it. Steff is both. | 120 |
| **Approach** | Two complementary sources. (1) Theory: TCE (frequency, asset specificity; uncertainty was blog 1) + cost per successful task, as a business case per task with four questions: may it be rented? how often? does it decompose into simple steps? break-even (setup ÷ saving per run, with the Gartner correction)? Task = sequence of steps with one acceptance criterion; evaluate the system per task. Make = own the process, buy the model as a commodity; buy = rent the capability. (2) Observation: what companies do. | 400 |
| **What happened: analysis** | The task table (below). Make wins exactly at the two conditions, and those tasks share one infrastructure. Break-even chart for routine work, with a second curve where AI lowers the set-up cost. Disappointment: per task, open models are 5–6x cheaper, not the 20x of the price list (Gartner); task complexity matters (OVH 3x Databricks). | 500 |
| **What happened: observation** | Companies ask the same question. Those who optimise decide per task and switch (Uber, AT&T, Revolut, Intercom, Pinterest, C3 AI); all-in firms often sell that expertise (Cognizant, Deloitte). Common factor: cheap switching, i.e. the gateway. | 300 |
| **Answer** | Worth building when benefits outweigh cost. Benefits stack because the infra is shared: disqualified tasks become possible; high-volume tasks that decompose into simple steps get cheaper; one sentence on insurance and leverage (blog 1). Cost: build and maintain, falling fast because AI builds it, so the threshold drops (TCE boundaries shift). Candidate line: make the process, buy the commodity, rent the frontier. | 350 |
| **What this doesn't show** | Whether the floor clears the bar per task; the real cost of building and maintaining. Company figures mostly self-reported. | 100 |
| **What next** | Build the gateway infrastructure and measure what it really costs. | 80 |
| | | **1,850** |

### Task table (starting point; Steff refines while writing)

| Task (from blog 1) | May it be rented? | How often? | Decomposes? | Break-even | Verdict |
|---|---|---|---|---|---|
| Building pipelines (clinical trials) | Yes | Low | Partly | No: human time dominates (real: ~$2.70 model vs ~1 h human) | Buy, fallback ready |
| Routine work: transformations, docs, tests | Yes | High, daily | Yes | TODO(steff): assumptions | Make |
| Brainstorming, exploring, infra code | Yes | Medium | No (new work) | Never | Buy (subscription) |
| Learning, small everyday tasks | Yes | Medium | Yes | Already cheap, low stakes | Cheapest (local) |
| Sensitive personal data | No | – | – | No choice | Make (required) |
| Scenario (hypothetical): a task in a product, 1,000 runs a day | Yes | Very high | Yes | TODO(steff): assumptions | Make |

Open for writing: TODO(steff) the scenario in more detail; the
assumptions behind the break-even numbers (mark them as illustrative; apply
the Gartner correction); check company figures against primary sources.

## Older rounds

## Structure (round 7, Steff)

Two complementary sources of evidence, in this order:
1. **Theoretical analysis**: the mental framework to understand the
   decision. TCE + cost per successful task, applied as a business case per
   task to Steff's own tasks (same task list as blog 1). Real anchor: the
   clinical-trials session; volume cases are marked assumptions; Gartner as
   the correction from price list to per-task saving.
2. **Observation**: how companies put this into practice, with cheap
   switching. They decide per task and switch (Uber, AT&T, Revolut, ...);
   all-in firms often sell that expertise. Common factor: a cheap switching
   function, i.e. the gateway.

Make = own the process (gateway, harness, evals, decomposed tasks), buy the
model as a commodity. Buy = rent the finished capability.
What next: build the gateway infrastructure and measure what it costs.

## Skeleton (round 5)

| Part | Content | Budget |
|---|---|---:|
| **Question** | "What is the business case for building the fallback?" Bridge: blog 1 convinced you of the supply risk; now the manager asks whether the fallback is worth it. Steff is both. | 120 |
| **Approach** | TCE at task level: frequency and asset specificity (uncertainty was blog 1); specificity answers Prahalad & Hamel (frequent but generic is not core). Task = sequence of steps with one acceptance criterion; each step to the cheapest executor that passes (code, small model, frontier, human); evaluate the system per task. Cost per successful task and break-even as the calculator. Rent builds nothing; the fallback is an investment. | 450 |
| **What happened** | *Start:* the clinical-trials session, a one-off: $16.90 model, ~1 h human, so renting wins (real). *Turning point:* break-even chart, cost per task vs runs per day, one-off to 1,000/day; the line moves when set-up cost falls (assumptions, marked). *Reality check:* companies that build meet the conditions (volume or filters); all-in firms often sell that expertise; Vercel 56% of tokens / 14% of spend. *Disappointments:* Gartner, 5–6x cheaper per task, not the 20x of the price list; OVH 3x Databricks. | 650 |
| **Answer** | Worth building when benefits outweigh cost. Benefits stack (shared infra): disqualified tasks become possible; high-volume tasks that decompose into simple steps get cheaper; plus one sentence on insurance and leverage (blog 1). Cost: build and maintain, falling fast because AI builds it, so the threshold drops (TCE boundaries shift). Candidate line: make the process, buy the commodity, rent the frontier. | 350 |
| **What this doesn't show** | Whether the floor clears the bar per task (untested). Company figures mostly self-reported. | 100 |
| **What next** | Cliffhanger: does the floor clear the bar on my tasks? The test. | 80 |
| | | **1,750** |

**Pointing back and forward.** Plant: the question picks up blog 1's
cliffhanger. Hinge: limits (the floor is untested). Cliffhanger: the test.

## What changed since the material was written

- Blog 1 no longer ends on "own the core, rent the edge". It ends on
  **fallback, not exit** (supply risk), with a cliffhanger: supply risk says
  *have* a fallback; what if cost says *use* it, and building it is an
  investment rather than rent? (blog 1 skeleton, round 3)
- Steff (2026-11-16): the cost post shows that companies can lower costs by
  moving core tasks to the floor, and argues that investing in e.g. gateway
  infrastructure is fundamentally different from paying rent to Anthropic.

## Questions found in the material

| # | Question | Where |
|---|---|---|
| Q1 | "When does it pay to own instead of rent?" | outline round 2; the draft's question |
| Q2 | "What is core, what is edge, and how do we decide that line quantitatively?" | outline round 3 (supersedes Q1; the draft was not updated) |
| Q3 | Under which conditions is each company strategy rational? | outline part 3; draft "When each company strategy makes sense" |
| Q4 | What should you invest in, given that capital depreciates fast? | draft lesson (fast vs slow depreciation table) |
| Q5 | Does a good harness lower the bar more for small models than for frontier models? | draft What next |

## Tensions

1. **The draft's own numbers undercut the floor claim.** With a harness,
   Claude drops from ~€28 to ~€11 and the open model lands at ~€9: "most of
   the gain comes from the harness, not from the model". Cost then barely
   separates ceiling and floor; what the harness buys is swappability.
2. **What happened is mostly assumptions.** Real data: the clinical-trials
   session ($16.90 model, ~1 h human, OVH ~3x Databricks). The bar (91%,
   1.5 minutes) and the harness case (40 h, 95%) are illustrative.

## Parking lot

- **Fast vs durable investments table** (draft lesson) → the post on building
  the fallback (harness, evals, gateway).
- **Company strategy table in full** → condensed into the reality check;
  full table can go to a concept page.
- **Decision theory footnotes** (consider-then-choose, penalty method,
  geometric retries) → cut; footnote at most.
- **Wardley mapping** → later in the series (frontier to commodity).
- **Gateway build data** → the post on building it (using it here is circular).
- **Q5, harness lowers the bar more for small models?** → the test post.

## Log

- 2026-11-16, round 1: five questions and two tensions read back.
- 2026-11-16, round 2: Steff: blog 1 = supply risk says have a fallback for
  core tasks; blog 2 = cost: when do you use what. Cost is complex: one cost
  builds something (infrastructure), the other is rent. Numbers confirm the
  model: the example was a one-off where development time dominated; at
  1,000 runs a day the logic flips, so make that clearer. Bridge: map the
  costs and benefits of the fallback; how does someone convinced by blog 1
  convince their manager? Steff is both the engineer and the manager.
- 2026-11-16, round 2: framework: transaction cost economics (Williamson):
  uncertainty (blog 1), frequency and asset specificity (blog 2). Applied to
  three tasks: building a pipeline (buy), routine work (make), a runtime task
  in the product (make). Candidate line: make the process, buy the commodity,
  rent the frontier. Weakness: the real example (pipeline build) is the
  low-frequency case; B/C have no measurement yet.
- 2026-11-16: Steff: a second assumption needs grounding: we evaluate
  everything at task level. This decides the eval strategy later.
- 2026-11-16, round 3: task = a sequence of steps with one acceptance
  criterion someone values; each step goes to the cheapest executor that
  passes (code, small model, frontier, human). Evaluate the system (code +
  model) at task level; steps for diagnosis. Small models look deceptively
  good because code carries much of the work.
- 2026-11-16, round 3: Steff: AI drives the cost of writing that code toward
  zero (e.g. Claude helped build the gateway). Insight kept: TCE still holds,
  but AI shifts its boundaries; making pays at much lower frequency. What
  remains of the investment: specifying, decomposing, verifying, noticing
  change.
- 2026-11-16, round 3: gateway build data NOT used: the gateway is not done,
  and this post argues economically for building it (using it would be
  circular). Back to the core question.
- 2026-11-16, round 4: question: "What is the business case for building
  the fallback?" Answer (Steff): disqualified tasks + high-volume simple
  tasks. Steff agreed to add: cost side with shifting boundaries, stacking
  benefits, decomposable instead of simple (in the answer); insurance and
  leverage as one sentence; the floor clearing the bar under limits.
- 2026-11-16, round 5: evidence agreed: session (start), chart (turning
  point), companies + Vercel (reality check), Gartner + OVH (disappointments).
  No new measurement: the test is the next post. Full skeleton written.
- 2026-11-16, round 6 (Steff on round 5): question good. Approach good, but
  the line of proof is unclear: the calculation and the company examples
  feel like two different methods, and make vs buy is not clear. What
  happened: confusing. What next: building the gateway infrastructure as the
  test, otherwise costs cannot be measured.
- 2026-11-16, round 6: proposed one method (business case per task, four
  questions) and make = own the process, buy the model as a commodity.
  Steff disagreed with parking the companies: they show that companies ask
  this question too, and they argue the need for a cheap switching function
  (the gateway).
- 2026-11-16, round 7: Steff: "the others" before the method breaks the
  flow. Structure = theoretical analysis, then observation of what companies
  do. Complementary: one gives the framework, the other shows the practice
  with cheap switching.
- 2026-11-16, round 8: task table accepted as a starting point. The
  1,000-a-day scenario is hypothetical; Steff will think it through in more
  detail while writing. Skeleton holds; next action: write (after blog 1).
