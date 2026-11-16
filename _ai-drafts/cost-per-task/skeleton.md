# Skeleton: blog 2 (what an AI task really costs)

> AI-written (exponential-post skill, move 1: from raw material). Material:
> `outline.md` (rounds 2–3 + material moved from post 1) and `draft.md`
> (AI draft, 2026-11-11). Both are material, not the answer.

## Status

Round 4: question and answer set. Next: what happened (the evidence).

## Skeleton (round 4, in progress)

| Part | Content |
|---|---|
| **Question** | "What is the business case for building the fallback?" (Steff). Bridge from blog 1: convinced of the supply risk, now convince the manager; Steff is both. |
| **Approach** | Transaction cost economics at task level: frequency and asset specificity (uncertainty was blog 1). Task = sequence of steps with one acceptance criterion; each step to the cheapest executor that passes (code, small model, frontier, human); evaluate the system per task. Cost formula and break-even as the calculator inside TCE. Rent vs investment. |
| **What happened** | TODO: decide the evidence (round 5). |
| **Answer** | Steff: "The fallback is worth building if: you have tasks that are disqualified from rented solutions; you have tasks that are cheaper with the infra, i.e. high volume simple tasks." Agreed additions: benefits stack because the infra is shared; "simple" means decomposable into simple steps; the cost side: build and maintain, falling fast because AI builds it, so the threshold drops (TCE boundaries shift); plus one sentence: insurance and leverage against supply risk (blog 1). |
| **What this doesn't show** | The floor must clear the bar per task; untested. |
| **What next** | The test: does the floor clear the bar? (cliffhanger) |

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
