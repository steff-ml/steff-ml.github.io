---
title: "Own the core, rent the edge: what an AI task really costs"
date: YYYY-MM-DD 09:00:00 +0200
categories: [build-log]
series: data-ai-engineering
chapter: stack
part: 2
quote: "Price is what you pay. Value is what you get."
quote_author: "Warren Buffett, letter to Berkshire Hathaway shareholders, 2008"
quote_source: "https://www.berkshirehathaway.com/letters/2008ltr.pdf"
concepts:
  - ai-gateway
excerpt: "Renting AI is cheap per task, because your own time dominates. Owning pays off through volume, constraints and capital that keeps its value, whichever model wins."
tags:
  - ai-engineering
  - ai-sourcing
---
{% comment %}
AI DRAFT (written with the exponential-post skill, based on the interview in
outline.md). Not reviewed yet. Check every claim, then move this file to
_drafts/ or _posts/ and delete this comment.

Open items:
- Every number marked "assumption" is illustrative. Replace with test results later.
- Currency: dollars and euros treated as roughly equal for the illustration.
- TODO: make the chart (cost per task vs number of tasks), e.g. with the dataviz skill.
- Opening quote replaces the cartoon. Attributed to the 2008 letter by secondary
  sources; check the wording in the letter itself.
  Alternatives: Rich Sutton, The Bitter Lesson (2019): "The biggest lesson that can
  be read from 70 years of AI research is that general methods that leverage
  computation are ultimately the most effective, and by a large margin."
  William Bruce Cameron (1963, often misattributed to Einstein): "Not everything that
  can be counted counts, and not everything that counts can be counted."
- Reference legend: (checked) = found by search on 2026-11-11; (notes) = your
  research notes; (memory) = from memory, check the link before publishing;
  (assumption) = my own illustrative number.
- TODO: link to post 1 and to the clinical-trials post.
- TODO(steff): confirm the human-time split (15 minutes for the Databricks task is my assumption, from your estimate of ~1 hour for the whole session).
- The open question in part 5 is my suggestion, not yet discussed.
- Case study figures come from post 1 and still need checking against primary sources.
{% endcomment %}

## The question

In the [previous post](TODO) I argued that relying fully on one frontier provider puts you in a rental relationship for the core of your business, and ended with a rule: **own the core, rent the edge.** I also ended with a doubt: is all this nuance worth it, or should we simply stay with the frontier models?

A rule is only useful if you know where the line is. So this post asks: **when does it pay to own instead of rent?**

## The approach

I kept the six criteria from the previous post (access, economics, performance, control, continuity, effort), but gave each a different role. The mental model has two steps:

```
task → hard filters → remaining options → lowest cost per successful task → pick, with a fallback
```

This two-step structure is not new. Decision research calls it "consider-then-choose": people first screen options with hard cut-offs, and only then trade off what remains.[^consider]

**Step 1: filter.** Some criteria rule options out for a task. Patient data that may not leave the EU rules out US APIs. A task that needs vision rules out text-only models. A client who demands local processing rules out every API.

**Step 2: minimise cost.** Among the options that remain, pick the one with the lowest cost per *successful* task. The criteria that don't filter get a price instead.

A criterion is not a filter or a cost by nature. It depends on the task: control is a hard filter for patient data and a small premium for a preference. Formally, a hard filter is just an infinite cost, the same trick optimisation uses when it turns a constraint into a penalty.[^penalty]

### The cost of a task

The cost of one successful task has three parts:

| Part | What it is | How to calculate it |
|---|---|---|
| **Running cost** | What each attempt costs, including your own time | (model cost + review time × hourly rate) / success rate |
| **Set-up cost** | One-off work, spread over every time the task runs | (engineering time × hourly rate + fixed costs) / number of tasks |
| **Risk** | What you lose if a source disappears or gets more expensive | probability × switching cost |

The split into running and set-up cost is classic break-even analysis: fixed costs divided by volume, plus variable costs per unit.[^breakeven] The model cost depends on how you get the model: tokens × price for pay-per-use, a fixed fee divided by the number of tasks for a subscription (up to its limits), a GPU hour divided by the tasks it handles for rented compute, and depreciation plus power plus maintenance for hardware you own. For rented and owned hardware, utilisation decides almost everything: published break-even estimates for the same GPU node range from a few percent to over half of its capacity, depending on which API you compare it with.[^gpu-breakeven]

Two things that usually stay vague become explicit here. **Quality** is the success rate: a cheaper model that fails half the time can cost more per task. Dividing by the success rate assumes you retry until it works; on average that takes 1 / success rate attempts.[^geometric] Benchmarks increasingly report this as cost per resolved task rather than cost per token.[^per-task] **Effort** is your time: if Claude does something in one go and a smaller model needs hours of tuning, those hours count.

Then there is one thing the formula does not capture by itself: some spending is not a cost but an investment. More on that below.

## What happened

### A real task: the clinical trials session

In the clinical trials session, Claude Code built the same data pipeline on three platforms after an interview at the start. Claude Code records the cost of each turn at API list price, so I could reconstruct what it cost:

| Phase | Model cost (API list price) |
|---|---|
| Interview | $0.50 |
| Build on Databricks, including verification | ~$2.70 |
| Same on Fabric / Power BI | ~$3.40 |
| Same on OVHcloud | ~$10.30 |
| **Total** | **$16.90** |

On a subscription I didn't pay this per task. But it is what the same work costs through an API, and it is about what a month of subscription costs, in one session.

My own time: about an hour of typing answers. At €100 per hour, that is about **€100 of human time against about €17 of model**. Roughly 86% of the cost of this session was me, even with the most expensive model available.

Two other things stood out:

- **The same task cost three times as much on OVHcloud** as on Databricks. The platform was less familiar, its documentation was not reachable from the session, and a dashboard tool needed fixing. Task complexity drives cost, not just the model.
- **Most tokens were not output.** Of about 38 million tokens, almost all were cache reads: in a long agent session, the model re-reads the whole context at every step. Cache reads are billed at a fraction of the normal input price,[^caching] but at this volume they still add up. Shorter, decomposed steps are cheaper.

### The bar for open models

This sets a bar that a cheaper model has to clear, per task. Take the Databricks task, with these assumptions: Claude succeeded in one go, the task took about 15 minutes of my time at €100 per hour, and an open model is ten times cheaper per task.[^assumptions-bar]

- Claude: (€2.70 + €25) / 1.0 ≈ **€27.70 per successful task**.
- Open model, same time from me: (€0.27 + €25) / success rate.

That gives two numbers:

| Bar | Value | Meaning |
|---|---|---|
| **Minimum success rate** | ~91% | Below this, the open model costs more per successful task, even at a tenth of the price |
| **Maximum extra human time** | ~1.5 minutes | If the open model needs more than about a minute and a half of extra steering, it loses |

A tenth of the price buys about a minute and a half of my time. As long as human time dominates, the token price barely matters.

### The same formula with an investment

Now assume I invest in the process instead of running it as a one-shot: a harness that splits the task into small steps, checks each step, and needs only a few minutes of my review. Assumptions: 40 hours of set-up (€4,000), review time down to 5 minutes, and a small open model succeeding on 95% of the decomposed steps.[^assumptions-harness] Splitting work into fixed steps is a known pattern: Anthropic describes prompt chaining as trading latency for accuracy, because each call has an easier job.[^chaining]

| | No harness | With harness |
|---|---|---|
| **Claude** | (€2.70 + €25) / 1.0 ≈ €27.70 | (€2.70 + €8.30) / 1.0 ≈ €11.00 + set-up |
| **Open model** | needs ≥91% success and ≤1.5 extra minutes | (€0.27 + €8.30) / 0.95 ≈ €9.00 + set-up |

Two conclusions, and the second one surprised me:

1. **The investment pays off through volume.** €4,000 of set-up is recovered after roughly 200 runs compared with the one-shot approach (€4,000 / (€27.70 − €9.00)). For a one-off task that never happens; for a task that is your daily work, it happens quickly.
2. **Most of the gain comes from the harness, not from the model.** This matches what others report about routing: in research, cascades and routers that send easy queries to cheaper models cut costs substantially at similar quality,[^routing] and in AT&T's case most of the savings are credited to the router rather than to the open models themselves.[^att-router] With a harness, Claude also drops from about €28 to about €11. The difference between Claude and the open model is then only about €2 per task. What the harness really buys is that the model becomes swappable: once the process is yours, choosing the model is a small decision you can revisit every few months.

{% comment %} TODO: chart, cost per task (y) vs number of tasks (x). Flat line: one-shot on Claude. Falling curves: harness with Claude, harness with an open model. Crossing point = when the investment pays off. {% endcomment %}

### When each company strategy makes sense

The previous post showed companies making opposite choices. With the formula, each one becomes rational under its own conditions:

| Strategy | Rational when | Example |
|---|---|---|
| **All-in on a frontier provider** | Tasks are varied and low-volume, human time dominates, there are no hard filters, and speed matters more than cost | Services firms such as Cognizant, whose expertise in one provider is also what they sell |
| **Both at once** | Volume grows faster than expected on uncontrolled frontier usage, and repeated tasks get moved | Uber, which reportedly used up its yearly coding budget in four months |
| **Open by default, frontier as exception** | High-volume, repeated tasks, an investment in infrastructure already made, and a capability filter for the exceptions | Revolut (vision goes to frontier), Intercom (one high-volume job moved) |
| **Own models** | Very high volume on a core process, plus control or cost constraints | Pinterest, C3 AI |

The aggregate data fits: on Vercel's AI Gateway, open-weight models handled 56% of tokens in August 2026 but only 14% of spend. Volume, the repeated work, moves to open models. The varied, high-value work stays with the frontier.

## The lesson

**Per task, renting is cheap, because your own time dominates. Owning pays off through volume and constraints, and only if you invest in a process that lets smaller models do the work.**

That is the quantitative side of "own the core, rent the edge". The core is the work that repeats, that falls under hard constraints, and that you can't afford not to understand. The edge is everything varied, exploratory and one-off.

Owning is also a different kind of spending. Rent is an operating cost: it builds nothing, and the landlord sets the price. Owning is a **capital investment**: it builds an asset. Whether to make or buy is an old question in economics; the classic answer is to keep in-house what is specific to your business and core to how you compete.[^make-or-buy] Capital is not better by definition, and there are many cases where renting wins. But a company that doesn't own its own processes has nothing left that a model can't take over.

Capital also depreciates, and in AI it can depreciate fast. A harness built around the quirks of today's small model can be obsolete in a year; general methods with more compute tend to beat hand-built structure, as Rich Sutton argued in "The Bitter Lesson".[^bitter] So the question is not whether to invest, but **in what**:

| Depreciates fast | Keeps its value, whichever model wins |
|---|---|
| Fine-tunes of one specific model | **Process knowledge**: your processes written down and split into steps a model can do |
| Prompts tuned to one model's quirks | **Evals**: task definitions, test sets and acceptance criteria. They are the bar above, made executable |
| Workarounds for one model's weaknesses | **Verification**: checks that don't depend on which model did the work |
| Hardware bought for today's model sizes | **Data contracts**: fixed structures for inputs and outputs |
| | **A gateway**: the ability to switch models with a configuration change. Its value is an option value: the value of being able to change course later[^options] |
| | **Usage logs**: cost and success per task, so you can recalculate the line when prices move |

The clinical trials session already contained a small piece of this: the interview produced a specification with acceptance criteria, and a verification step checked each one. That is capital that survives a model switch.

For me, the line is fairly clear. Data engineering is, like software engineering, logic-based and regular: every project needs pipelines, CI/CD and, increasingly, regulatory data contracts with a fixed structure. That is a lot of repeated work that relatively simple models can probably do. Understanding a new domain and verifying results are harder, and I expect them to stay with frontier models and with me. Add clients who require European or even local processing, and investing in my own process becomes a precondition, not a choice. That is my hypothesis; the tests still have to confirm it.

## What next

The previous post asked whether all this nuance is worth it. The answer: **for one-off work, no; for your core, yes.** Stay on the frontier for whatever is varied and exploratory, and invest in what keeps its value for whatever repeats.

The next step is to build that capital: a harness with evals, verification and a gateway, so I can test whether open models actually clear the bar.

The question I want that to answer: **does a good harness lower the bar more for small models than for frontier models?** If it does, the real gain of the exponential isn't the next model. It's owning the process that lets you use any of them.

[^consider]: (memory) John Hauser and Birger Wernerfelt, "An Evaluation Cost Model of Consideration Sets", Journal of Consumer Research 16(4), 1990; for the idea of screening before optimising, see also Herbert Simon, "A Behavioral Model of Rational Choice", Quarterly Journal of Economics, 1955.
[^penalty]: (memory) See [penalty method](https://en.wikipedia.org/wiki/Penalty_method) in constrained optimisation.
[^geometric]: (memory) The number of attempts until the first success follows a [geometric distribution](https://en.wikipedia.org/wiki/Geometric_distribution), with mean 1 / p. This assumes attempts are independent and a failed attempt costs as much as a successful one.
[^per-task]: (checked) For example [Faros AI](https://www.faros.ai/blog/open-models-vs-frontier-models) compares open and frontier coding routes per task; [Towards Data Science](https://towardsdatascience.com/the-hidden-cost-of-llm-routing-why-switching-models-costs-more/) reports cost per success.
[^breakeven]: (memory) See [break-even analysis](https://en.wikipedia.org/wiki/Break-even_(economics)).
[^gpu-breakeven]: (checked) [Self-Hosted LLM vs API Cost (DEV)](https://dev.to/alanwest/self-hosted-llm-vs-api-cost-below-105k-a-month-an-h200-node-cant-beat-the-api-20) · [Self-Hosted LLM vs API: The Real Break-Even](https://www.rakibulinux.com/blogs/self-hosted-llm-vs-api-cost-break-even) · [Own the Base, Rent the Peak](https://getnadir.com/blog/self-hosted-llm-vs-api-break-even-hybrid-routing/). Vendor and practitioner analyses; the thresholds differ because the assumptions differ.
[^caching]: (memory) Anthropic, [prompt caching](https://platform.claude.com/docs/en/build-with-claude/prompt-caching). TODO: check the current price ratio.
[^assumptions-bar]: (assumption) €100/h is a round illustrative rate. 15 minutes: my share of the ~1 hour I spent in the whole session. Ten times cheaper: in line with company reports such as [Revolut](https://ai.engineer/talks/Ok6QdZHCcI8-revolut-cut-ai-coding-costs-90-self) (about a tenth of the cost) and [Pinterest](https://consciousengines.com/blog/frontier-model-downshift-index) (below 8% per transaction), and lower than the per-token gap because open models tend to use more tokens and retries ([Gartner via The Register](https://www.theregister.com/ai-and-ml/2026/11/03/open-models-can-do-90-of-agentic-work-at-a-twentieth-of-the-cost-of-a-frontier-llm-gartner/5307132)).
[^assumptions-harness]: (assumption) 40 hours, 5 minutes and 95% are illustrative, not measured. Set-up can be far larger: one vendor reports a fintech client spending 6 weeks and $38,000 of engineering time scaling a self-hosted Mistral deployment ([Braincuber](https://www.braincuber.com/blog/self-hosted-llms-vs-api-based-llms-cost-performance-analysis)). With €38,000 of set-up instead of €4,000, the break-even moves from ~200 to ~2,000 runs.
[^chaining]: (checked) Anthropic, [Building effective agents](https://www.anthropic.com/engineering/building-effective-agents). See also Berkeley AI Research, [The Shift from Models to Compound AI Systems](https://bair.berkeley.edu/blog/2024/02/18/compound-ai-systems/) (memory).
[^routing]: (memory) Lingjiao Chen, Matei Zaharia and James Zou, [FrugalGPT](https://arxiv.org/abs/2305.05176), 2023; Isaac Ong et al., [RouteLLM](https://arxiv.org/abs/2406.18665), 2024.
[^att-router]: (checked) [13 Companies Replacing Frontier AI Models](https://consciousengines.com/blog/frontier-model-downshift-index) (commentary, not AT&T itself).
[^make-or-buy]: (memory) Ronald Coase, "The Nature of the Firm", 1937; Oliver Williamson, *The Economic Institutions of Capitalism*, 1985; C.K. Prahalad and Gary Hamel, ["The Core Competence of the Corporation"](https://hbr.org/1990/05/the-core-competence-of-the-corporation), Harvard Business Review, 1990.
[^bitter]: (notes) Rich Sutton, [The Bitter Lesson](http://www.incompleteideas.net/IncIdeas/BitterLesson.html), 2019.
[^options]: (memory) Avinash Dixit and Robert Pindyck, *Investment under Uncertainty*, 1994; Carliss Baldwin and Kim Clark, *Design Rules: The Power of Modularity*, 2000.
