---
title: "Resistance is not futile: fallback options"
date: 2026-11-22 09:00:00 +0200
categories: [build-log]
series: data-ai-engineering
chapter: stack
part: 1
quote: "Strength is irrelevant. Resistance is futile. We wish to improve ourselves. We will add your biological and technological distinctiveness to our own. Your culture will adapt to service ours."
quote_author: "Anthropic at AWS Summit Washington, D.C. 2027"
quote_source: "https://memory-alpha.fandom.com/wiki/The_Best_of_Both_Worlds_(episode)#Memorable_quotes"
concepts:
  - ai-sourcing
excerpt: "Claude can do a lot of my work. What does that dependence mean for my career or business, and which other ways of getting AI reduce the risk?"
tags:
  - ai-engineering
  - ai-sourcing
---


## Introduction



## There are many methods to source AI
### AI model subscriptions


### API based billing


### Renting computers in the cloud and running the models there


### Running models locally



## The lesson: Small open-weight models win on supply chain risk.


## One move that benefits twice: assigning tasks to the right model decreases dependence AND cost


## Conclusion
- In this blog post, I set out to understand how I could manage AI supply chain risks using Kraljic's matrix from the previous blog post
- After studying the different options, the risk differentiator turned out to not be HOW the model was sourced (API, subscription, ...), but what model was sourced (big/closed vs small/open).
- I found a simple lesson: Effectively managing supply chain risk is about moving tasks from intelligent, but risky models to widely available, but less intelligent models.
- Real-world companies manage both supply risk and cost by routing the right tasks to the right model.
- In the next post, I'll discuss how to build such a routing system.




### Measuring supply risk

AI can be bought in five ways. What changes from one to the next is what you buy, and whether anyone else could sell you the same thing:

1. **Subscription**: you buy a finished product, apps and model together, for a flat monthly fee. The provider decides which model you get and how much you may use.
2. **API, closed model**: you buy answers from one lab's model, per token, in your own software. A hyperscaler is another shop for the same model, not another model.
3. **API, open model**: the same invoice, but the model has no single owner. Many providers serve the same weights, so you can switch.
4. **Rented compute**: you buy GPU time instead of answers and run the model yourself. You pay whether the GPU is busy or not.
5. **Own hardware**: you buy the machine. Nobody can cut you off, and every risk the provider used to carry is now yours.

This lead me to the following evaluation:

| | Subscription | API, closed model | API, open model | Rented compute | Own hardware |
|---|:---:|:---:|:---:|:---:|:---:|
| **Disqualifiers** (data, access, usage policy) | ✗ | ✗ | ~ | ✓ | ✓ |
| **Supply risk** (sellers of the same model) | ✗ 1 | ✗ 1 | ✓ many | ✓ many | ✓ you |
| **Capability** | ✓ | ✓ | ~ | ~ | ✗ |
| **Effort** | ✓ | ✓ | ✓ | ✗ | ✗ |
| **Cost** | ✓ | ~ | ✓ | ~ | ~ |

✓ = strong, ~ = depends on provider, volume or use case, ✗ = weak. Personal judgement and a simplification.

For the detailed scoring and more on each option:  [AI sourcing options]({{ '/ai-sourcing-options/' | relative_url }}).

This table illustrates that the dividing line is between **big, closed-weight models** and **small, open-weight models**. Big, closed-weight models such as Claude score well on both effort and capability, but introduce supply-chain and disqualification risks because there are only a limited suppliers of them. Small, open-weight models trail the big ones in capability, but have many independent providers and can be run on your own hardware to handle any disqualifiers. 



### Measuring impact

{% comment %} TODO(steff): check every placement. Core vs edge is your call; it depends on what you want to earn your money with. {% endcomment %}

In the [previous post](TODO) Claude did real data engineering work for me. Placing that work and the rest of my AI use in the matrix gives this:

| Task | Core or edge | Ceiling or floor | Quadrant | What follows |
|---|---|---|---|---|
| Building data pipelines, as in the clinical trials post | Core | Ceiling, for now: I haven't tested the floor yet | Strategic | Build the fallback: test the same tasks on open models |
| Routine production work: transformations, documentation, tests | Core | Floor | Leverage | Run it on Qwen at several EU providers and switch when it pays |
| Brainstorming, exploring new topics, infrastructure code | Edge | Ceiling | Bottleneck | Use the Claude subscription. Nothing in production depends on it |
| Learning, small everyday tasks | Edge | Floor | Non-critical | Small models on my own hardware |
| Anything with sensitive personal data | Disqualified for external APIs | | | Own hardware, or an EU provider where the contract allows it |

This answers the question I started with. **Should Claude do all the data engineering it is capable of? For the edge, yes, freely. For the core, only with a way out.** Two things surprised me:

- **I don't have to diversify everything.** Depending on the frontier for edge work is fine. If it disappears tomorrow, I lose convenience, not income.
- **For core work, the answer is not to stop using Claude.** It is to make sure Claude is not the *only* thing that can do it. The strategic quadrant asks for a fallback, not an exit.

My own portfolio follows from the table:

{% comment %} TODO(steff): revise rows, mark what is in use and what is planned. {% endcomment %}

| Way of getting AI | Provider / model | Used for | Quadrant |
|---|---|---|---|
| **Subscription** | Claude | Brainstorming, infrastructure code, the frontier side of strategic work | Bottleneck, strategic |
| **API, open model** | Qwen via OVHcloud and Scaleway | All-round production work | Leverage, and the fallback for strategic work |
| **API, open model** | DeepSeek via Scaleway | Cheap long-context coding and review | Leverage, but with fewer providers, so nothing critical |
| **API, closed / partly open** | Mistral | Work where a European provider is required | Passing the disqualifiers |
| **Own hardware** | Small open-weight models | Learning, routine tasks, sensitive data | Non-critical, and the fallback of last resort |

### Why the floor, and not the ceiling

Look back at my own incidents in the table above. Every one of them hit the ceiling, not the floor. Fable and Mythos were switched off, and Mythos came back only for approved US organisations, after its preview had gone to a small group of large partners first.[^glasswing] My biology question was moved from the newest model to an older one.

I am not judging whether these rules are right. The pattern is what matters: **when capability is rationed, it is the ceiling that gets rationed.** Nobody restricts last year's open-weight model. It is already on thousands of servers.

And the floor rises. According to Epoch AI, the best open-weight models trail the closed frontier by only about four months.[^epoch] What needs the ceiling today runs on the floor next year. So the strategic quadrant is not a permanent place. A task leaves it as soon as the floor can do it, *if* I am ready to move it.

### What this doesn't show

**Whether the floor can do my core work.** The matrix says my pipeline work should have a fallback on open models. It doesn't say that fallback works. That is a capability question, like the one in the previous post, and I haven't tested it yet.

**Whether the danger is big.** My losses were minimal, but it did spook me. The case rests on what *could* happen. That's how insurance works, but it is a weaker argument than a disaster.

**What the move costs.** Running core work on the floor isn't free. Smaller models need tasks cut into smaller steps, and checks around them.

## The lesson

**The core should run on the floor of intelligence, not the ceiling.**

The ceiling is where the newest capability lives, and it is worth using: for exploring, for edge work, and to get core work going. But the ceiling has few suppliers, and few suppliers set the terms. The floor has many, and it keeps rising.

So use the frontier freely where losing it costs you convenience. Where losing it costs you income, make sure the work can also run on a model nobody can take away from you.

That holds at two levels:

- **For a business:** the processes you earn your money with should not depend on one lab.
- **For a career:** the work clients pay you for should not depend on one subscription.
