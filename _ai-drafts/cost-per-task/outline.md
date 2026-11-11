# Outline post 2: What does an AI task really cost?

> AI-written outline from the exponential-post interview. In progress, not final.
> Follows post 1 (`_ai-drafts/model-sourcing/outline.md`).

Character: quantitative. How to calculate it, and under which conditions the
company strategies from post 1 are a good choice. Plus the worked example.

## 1. The question
- Post 1 showed companies making opposite choices. When is which one rational?
- For me: is investing in open-source data + AI tooling justified?

## 2. The approach
- Cost model, which also defines the criteria I will judge models on.
  Clarity of presentation still to work out.

### Cost model (draft)
Operating cost:
cost per successful task =
  (model cost per attempt + human review time x hourly rate) / success rate
  + (setup/tuning engineering time x hourly rate + fixed costs) / number of tasks
  + risk premium (P(disruption) x switching cost)
- Hourly rate for illustration: EUR 100/h.
- Model cost per attempt by option: tokens x price (pay-per-use); fixed fee / N up to
  the rate limit (subscription); GPU hour / (tasks per hour x utilisation) (renting);
  (depreciation + power + maintenance) / N (ownership).
- The six criteria map onto the formula: access = candidate set; control = hard
  filter; performance = filter + success rate; economics = model cost; effort =
  engineering and review time; continuity = risk premium.
- Routing: filter on constraints, pick lowest cost per successful task, keep a fallback.

Investment (fixes "the formula punishes small models"):
- Harness, evals, decomposed processes, gateway = one-off investment that changes
  the parameters of all future tasks (higher success rate for small models, less
  review time, lower switching cost).
- Return ~ (cost per task before - after) x future tasks - investment.
- Decomposing a task raises reliability and lowers cost; small models are bad at
  undecomposed tasks, not at tasks.
- Process knowledge and swappability are capital. Swappability has option value:
  it lets you capture each step on the exponential curve. Grows with volume and
  with the speed of the curve. No single number yet.

## 3. What happened
### Worked example: the clinical-trials session (2026-11-08)
Cumulative cost at API list price (Claude Code result events):
| Phase | Wall clock | Cost |
|---|---|---|
| Interview (grilling, Q1-Q24) | 09:31-09:43 | $0.50 |
| Build on Databricks + verify | 09:43-09:55 | ~$2.7 |
| Fabric / Power BI (interview + build) | 09:57-10:18 | ~$3.4 |
| OVHcloud (interview + build + Superset) | 10:27-13:25 | ~$10.3 |
| Total | ~4 h | $16.90 (Opus $16.71, Haiku $0.19) |
- Human time: ~1 hour typing answers (Steff's estimate) = ~EUR 100 vs ~EUR 16 model:
  ~86% human, ~14% model.
- Volume is mostly cache reads (37.6M tokens), not output (~205k): a long agent
  session re-reads its whole context at every step. Argument for decomposition.
- Same task cost ~3x more on OVH than Databricks. Task complexity (less known
  platform, documentation not reachable, Superset error) drives cost.
- One session at list price ~ one month of subscription: subscriptions are cheap
  for heavy agentic work, within limits.
- Under which conditions each company strategy from post 1 is rational.
  TODO: work out per strategy.

## 4. The lesson
- Human time and task decomposition weigh more than the token price.
- One-shots ride on the frontier; systems capture the curve.
- Candidate core: frontier to explore, open models once something becomes repeatable.
- Case for me: if data engineering tasks become my bread and butter, investing in
  open-source technology is justified.
  TODO(steff): how repetitive are they (N)?
- Suggestion: "pin the formula, not the answer".

## 5. What next
- Planned post: turning repeated one-shots into well-understood systems with
  harness, evals etc. "The real gain comes when you have that."
- TODO(steff): open question.
