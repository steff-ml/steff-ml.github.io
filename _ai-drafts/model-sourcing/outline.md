# Outline post 1: The microeconomics of AI (rework of model sourcing)

> AI-written outline from the exponential-post interview. In progress, not final.
> Source post: `_posts/build-log/2026-10-24-model-sourcing.md` (left unchanged).
> Decision: split into two posts (option B). Post 2: `_ai-drafts/cost-per-task/outline.md`.

Series: Data + AI engineering on the exponential.
Comes after the clinical-trials post (Claude Code + grill-me ingests, cleans and
visualises clinical trials without further instructions).
Character: qualitative. Case studies show companies are actively making this choice.

## 1. The question
- The clinical-trials experiment works, but makes me dependent on Claude.
- What other ways are there to get AI, and what are the pros and cons?

## 2. The approach
- Desk research: the "microeconomics of AI".
- All ways to use and integrate AI (subscription, pay-per-use, renting, ownership),
  scored on six criteria (access, economics, performance, control, continuity, effort).
- Case studies of companies making very different choices (see spectrum below).
- Testing on real tasks needs evals + a gateway: out of scope.

## 3. What happened
- The scoring table (may still change).
- Spectrum of company strategies, from all-in frontier to own models.
- My portfolio: partly in use, partly planned; tests still have to run.
  Steff will revise the rows himself.
- Disappointment candidate: what can't be decided on paper (success rate per
  model per task is unknown without evals).
- Not really a disappointment, but a creeping feeling: maybe this doesn't matter and
  we can just stay with frontier models. Personal experience: people who discover
  the power of Claude suddenly feel much more intelligent and even start looking
  down on understanding things themselves. We developed a nuanced view on AI
  sourcing, but I don't know if it will prove worth it in hindsight.
  Suggestion: use this doubt honestly in part 3 and as the open question in part 5.

## 4. The lesson
- There is more to it than smarter = better.
- Companies make opposite choices, and both can be rational. Many route per task.
- Red thread (suggestion): today's best model is soon a commodity, so invest in
  being able to switch, not in choosing.

## 5. What next
- When is which strategy right? That needs numbers -> post 2 (cost per task).

## Accepted suggestions
- Move the long per-option reference material (providers, links, hardware) to a
  concept page, e.g. "AI sourcing options", and link from the post.

## Case study spectrum (to verify against primary sources)
All-in on a frontier provider:
- Cognizant: Claude group of 25,000+ people, Claude across its whole workforce
  (services firm, so partly a commercial bet).
- Deloitte: Claude Center of Excellence, 15,000 professionals certified. Not exclusive.
- Sierra: engineers self-estimate 3-20x productivity leaning heavily on Claude Code.
Mixed / tension inside one company:
- Uber: reportedly burned its 2026 AI coding-tools budget in 4 months (~5,000
  engineers on Claude Code); also moved some tasks to open models (AI costs down ~50%).
- Microsoft Copilot: Anthropic models on by default, multi-model orchestration.
Hybrid with a router:
- Revolut: self-hosted GLM for coding agents, router sends vision to frontier;
  "GLM is the normal path, frontier is the exception"; ~10x cheaper (company-reported).
- Intercom: one high-volume summarisation job (~$250k/month) to fine-tuned Qwen 14B;
  hardest customer-facing prompt stays on frontier.
- AT&T: up to 90% cost cut on some applications, reportedly 56% lower bill for a
  2% quality drop; savings credited to routing.
Open / own models:
- Pinterest: open models at <8% of the cost per transaction of closed models.
- C3 AI: fine-tunes its own models despite strong frontier models.
Background: Gartner (open models more verbose, per-task saving ~5-6x not ~20x);
Vercel gateway: open-weight token share 7% -> 56% (Dec -> Aug).
