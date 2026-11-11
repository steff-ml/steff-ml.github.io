---
title: "Own the core, rent the edge: what an AI task really costs"
date: YYYY-MM-DD 09:00:00 +0200
categories: [build-log]
series: data-ai-engineering
chapter: stack
part: 2
image: /assets/images/posts/TODO
image_alt: "TODO"
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
- TODO: cartoon (image, image_alt).
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

**Step 1: filter.** Some criteria rule options out for a task. Patient data that may not leave the EU rules out US APIs. A task that needs vision rules out text-only models. A client who demands local processing rules out every API.

**Step 2: minimise cost.** Among the options that remain, pick the one with the lowest cost per *successful* task. The criteria that don't filter get a price instead.

A criterion is not a filter or a cost by nature. It depends on the task: control is a hard filter for patient data and a small premium for a preference. Formally, a hard filter is just an infinite cost.

### The cost of a task

The cost of one successful task has three parts:

| Part | What it is | How to calculate it |
|---|---|---|
| **Running cost** | What each attempt costs, including your own time | (model cost + review time × hourly rate) / success rate |
| **Set-up cost** | One-off work, spread over every time the task runs | (engineering time × hourly rate + fixed costs) / number of tasks |
| **Risk** | What you lose if a source disappears or gets more expensive | probability × switching cost |

The model cost depends on how you get the model: tokens × price for pay-per-use, a fixed fee divided by the number of tasks for a subscription (up to its limits), a GPU hour divided by the tasks it handles for rented compute, and depreciation plus power plus maintenance for hardware you own.

Two things that usually stay vague become explicit here. **Quality** is the success rate: a cheaper model that fails half the time can cost more per task. **Effort** is your time: if Claude does something in one go and a smaller model needs hours of tuning, those hours count.

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
- **Most tokens were not output.** Of about 38 million tokens, almost all were cache reads: in a long agent session, the model re-reads the whole context at every step. Shorter, decomposed steps are cheaper.

### The bar for open models

This sets a bar that a cheaper model has to clear, per task. Take the Databricks task, with these assumptions: Claude succeeded in one go, the task took about 15 minutes of my time, and an open model is ten times cheaper per task.

- Claude: (€2.70 + €25) / 1.0 ≈ **€27.70 per successful task**.
- Open model, same time from me: (€0.27 + €25) / success rate.

That gives two numbers:

| Bar | Value | Meaning |
|---|---|---|
| **Minimum success rate** | ~91% | Below this, the open model costs more per successful task, even at a tenth of the price |
| **Maximum extra human time** | ~1.5 minutes | If the open model needs more than about a minute and a half of extra steering, it loses |

A tenth of the price buys about a minute and a half of my time. As long as human time dominates, the token price barely matters.

### The same formula with an investment

Now assume I invest in the process instead of running it as a one-shot: a harness that splits the task into small steps, checks each step, and needs only a few minutes of my review. Assumptions: 40 hours of set-up (€4,000), review time down to 5 minutes, and a small open model succeeding on 95% of the decomposed steps.

| | No harness | With harness |
|---|---|---|
| **Claude** | (€2.70 + €25) / 1.0 ≈ €27.70 | (€2.70 + €8.30) / 1.0 ≈ €11.00 + set-up |
| **Open model** | needs ≥91% success and ≤1.5 extra minutes | (€0.27 + €8.30) / 0.95 ≈ €9.00 + set-up |

Two conclusions, and the second one surprised me:

1. **The investment pays off through volume.** €4,000 of set-up is recovered after roughly 200 runs compared with the one-shot approach (€4,000 / (€27.70 − €9.00)). For a one-off task that never happens; for a task that is your daily work, it happens quickly.
2. **Most of the gain comes from the harness, not from the model.** With a harness, Claude also drops from about €28 to about €11. The difference between Claude and the open model is then only about €2 per task. What the harness really buys is that the model becomes swappable: once the process is yours, choosing the model is a small decision you can revisit every few months.

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

Owning is also a different kind of spending. Rent is an operating cost: it builds nothing, and the landlord sets the price. Owning is a **capital investment**: it builds an asset. Capital is not better by definition, and there are many cases where renting wins. But a company that doesn't own its own processes has nothing left that a model can't take over.

Capital also depreciates, and in AI it can depreciate fast. A harness built around the quirks of today's small model can be obsolete in a year; general methods with more compute tend to beat hand-built structure. So the question is not whether to invest, but **in what**:

| Depreciates fast | Keeps its value, whichever model wins |
|---|---|
| Fine-tunes of one specific model | **Process knowledge**: your processes written down and split into steps a model can do |
| Prompts tuned to one model's quirks | **Evals**: task definitions, test sets and acceptance criteria. They are the bar above, made executable |
| Workarounds for one model's weaknesses | **Verification**: checks that don't depend on which model did the work |
| Hardware bought for today's model sizes | **Data contracts**: fixed structures for inputs and outputs |
| | **A gateway**: the ability to switch models with a configuration change |
| | **Usage logs**: cost and success per task, so you can recalculate the line when prices move |

The clinical trials session already contained a small piece of this: the interview produced a specification with acceptance criteria, and a verification step checked each one. That is capital that survives a model switch.

For me, the line is fairly clear. Data engineering is, like software engineering, logic-based and regular: every project needs pipelines, CI/CD and, increasingly, regulatory data contracts with a fixed structure. That is a lot of repeated work that relatively simple models can probably do. Understanding a new domain and verifying results are harder, and I expect them to stay with frontier models and with me. Add clients who require European or even local processing, and investing in my own process becomes a precondition, not a choice. That is my hypothesis; the tests still have to confirm it.

## What next

The previous post asked whether all this nuance is worth it. The answer: **for one-off work, no; for your core, yes.** Stay on the frontier for whatever is varied and exploratory, and invest in what keeps its value for whatever repeats.

The next step is to build that capital: a harness with evals, verification and a gateway, so I can test whether open models actually clear the bar.

The question I want that to answer: **does a good harness lower the bar more for small models than for frontier models?** If it does, the real gain of the exponential isn't the next model. It's owning the process that lets you use any of them.
