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
- NEW (in discussion): a cost-per-task model to make the routing logic less qualitative.

## 3. What happened
- The scoring table (may still change).
- What the case studies show companies do differently.
- My portfolio: partly in use, partly planned; tests still have to run.
  TODO(steff): mark which rows are in use and which are planned.
- Disappointment candidate: what can't be decided on paper (success rate per
  model per task is unknown without evals).
  TODO(steff): anything else that surprised or disappointed.

## 4. The lesson
- Agreed: there is more to it than smarter = better, based on case studies,
  as long as infrastructure and evals aren't ready.
- Angle (suggestion, accepted): companies route per task instead of picking one model.
- Red thread (suggestion): today's best model is soon a commodity, so invest in
  being able to switch, not in choosing.
- In discussion: "pin the formula, not the answer" (cost per task).

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
