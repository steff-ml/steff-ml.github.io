# Outline: AI model sourcing (rework)

> AI-written outline from the exponential-post interview. In progress, not final.
> Source post: `_posts/build-log/2026-10-24-model-sourcing.md` (left unchanged).

Series: Data + AI engineering on the exponential.
Comes after the clinical-trials post (Claude Code + grill-me ingests, cleans and
visualises clinical trials without further instructions).

## 1. The question
- The clinical-trials experiment works, but makes me dependent on Claude.
- What are my other options to get AI, and when do I pick which one?

## 2. The approach
- Desk research, not tests: the "microeconomics of AI".
- All ways to use and integrate AI (subscription, pay-per-use, renting, ownership),
  scored on six criteria (access, economics, performance, control, continuity, effort).
- Case studies of companies (Revolut and others, see candidates below).
- Testing on real tasks needs evals + a gateway: out of scope, later posts.
- Cost-per-task model, in the post itself. It defines the criteria I will judge models on.
  Clarity of presentation still to work out.

### Cost model (draft)
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
- Volume decides: one-off task -> frontier (setup ~0); repeated task -> tuning an
  open model can pay off (Intercom story).
- Routing: filter on constraints, pick lowest cost per successful task, keep a fallback.

## 3. What happened
- The scoring table (may still change).
- What the case studies show companies do differently.
- My portfolio: partly in use, partly planned; tests still have to run.
  TODO(steff): mark which rows are in use and which are planned.
- Disappointment candidate: what can't be decided on paper (success rate per
  model per task is unknown without evals).
  TODO(steff): anything else that surprised or disappointed.

### Worked example: the clinical-trials session (2026-11-08)
Cumulative cost at API list price (Claude Code result events):
| Phase | Wall clock | Cost |
|---|---|---|
| Interview (grilling, Q1-Q24) | 09:31-09:43 | $0.50 |
| Build on Databricks + verify | 09:43-09:55 | ~$2.7 |
| Fabric / Power BI (interview + build) | 09:57-10:18 | ~$3.4 |
| OVHcloud (interview + build + Superset) | 10:27-13:25 | ~$10.3 |
| Total | ~4 h | $16.90 (Opus $16.71, Haiku $0.19) |
- Volume is mostly cache reads (37.6M tokens), not output (~205k).
- At EUR 100/h, human time likely dominates (~90% for the Databricks task, if ~15 min active).
  TODO(steff): actual active time per phase.
- Same task cost ~3x more on OVH than Databricks: platform/documentation is a cost factor.
  TODO(steff): why OVH was more expensive.
- One session at list price ~ one month of subscription: subscriptions are cheap for
  heavy agentic work, within limits.

## 4. The lesson
- Agreed: there is more to it than smarter = better, based on case studies,
  as long as infrastructure and evals aren't ready.
- Angle (suggestion, accepted): companies route per task instead of picking one model.
- Red thread (suggestion): today's best model is soon a commodity, so invest in
  being able to switch, not in choosing.
- In discussion: "pin the formula, not the answer" (cost per task).
- Candidate core: frontier to explore, open models once something becomes repeatable.
- Suggestion: make cost per task the spine (and maybe the title) of the post.

## 5. What next
- Gateway + evals to measure the parameters of the cost model.
- TODO(steff): open question.

## Accepted suggestions
- Move the long per-option reference material (providers, links, hardware) to a
  concept page, e.g. "AI sourcing options", and link from the post.

## Case study candidates (to verify against primary sources)
- Revolut: self-hosted GLM for coding agents, router sends vision to frontier
  models; "GLM is the normal path, frontier is the exception"; ~10x cheaper
  (company-reported, conference talk by David Abitbol).
- Intercom: one high-volume summarisation job moved to a fine-tuned 14B Qwen;
  hardest customer-facing prompt stays on a frontier model (per task, not per platform).
- Pinterest: open models at <8% of the cost per transaction of closed models (own statement).
- AT&T: up to 90% cost cut on some applications; savings credited to routing.
- Uber: open models for certain tasks, AI costs reportedly down 50%.
- Gartner: open models more verbose / retry more, so per-task saving ~5-6x
  instead of the per-token gap.
