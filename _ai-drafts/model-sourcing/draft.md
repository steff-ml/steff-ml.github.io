---
title: "Rent or own? The microeconomics of getting AI"
date: YYYY-MM-DD 09:00:00 +0200
categories: [build-log]
series: data-ai-engineering
chapter: stack
part: 1
image: /assets/images/posts/independent-ai-stack-cartoon.jpg
image_alt: "Cartoon of a signalman in a signal box labelled Independent AI stack, switching a train of prompts and data onto one of three tracks: a large frontier models station with a lowered barrier, a regional station for European cloud providers, and a small self-hosted station next to a cabin with a vegetable garden."
concepts:
  - ai-gateway
excerpt: "Claude can build a data pipeline on its own. That's great, and it's also a dependency. What are the other ways to get AI, and when do you rent instead of own?"
tags:
  - ai-engineering
  - ai-sourcing
---
{% comment %}
AI DRAFT (written with the exponential-post skill, based on the interview in
outline.md). Not reviewed yet. Check every claim, then move this file to
_drafts/ or _posts/ and delete this comment.

Open items:
- TODO(steff): revise the portfolio rows (in use vs planned).
- TODO(steff): confirm "what Claude doesn't have" in part 4 (my guess, not yet confirmed).
- TODO: verify every case study figure against a primary source.
- Sources added 2026-11-11: footnotes marked (notes) come from your research notes; the others are new and not yet opened.
- TODO: create the concept page "AI sourcing options" (providers, links, hardware)
  and replace the placeholder link.
- TODO: link to the clinical-trials post once it exists.
{% endcomment %}

## The question

In the [previous post](TODO) I let Claude Code ingest, clean and visualise clinical trial data, with nothing more than an interview at the start. It worked remarkably well, and I enjoy working with it.

It also made something clear: the more I let Claude do, the more my work depends on one provider. So this post asks a simple question: **what other ways are there to get access to AI models, and what are their pros and cons?**

## The approach

This post is desk research, not a test. Comparing models on real tasks needs evaluations and a gateway to switch between them, and that is for later posts. Here I wanted the bigger picture first: the microeconomics of AI.

**AI as a utility.** The usual way to think about AI is as a very smart colleague, which suggests that smarter is always better. I found it more useful to think of AI like electricity. A business draws power from the grid, but it also thinks about batteries and generators, and about what happens when the grid goes down or the price goes up.

That framing gave me six criteria:

| Criterion | Core question |
|---|---|
| **Access** | Which models can I use, and how soon after release? |
| **Economics** | What do I pay, and who carries the risk of over- or underuse? |
| **Performance** | Is it available, fast and consistent? |
| **Control** | Where does my data go, under which jurisdiction, and what do the terms allow? |
| **Continuity** | Can I switch cheaply? How likely is the provider to change terms or disappear? |
| **Effort** | What do I have to build and maintain myself? |

I scored the four ways to get a model on these criteria, from least to most hands-on:

1. **Subscription**: a monthly plan from a lab such as Anthropic, Google or OpenAI, or a plan with an API key for open-weight models.
2. **Pay-per-use**: an API billed per token, from a model lab, a hyperscaler, a European inference provider or Mistral.
3. **Renting compute**: renting GPUs in the cloud and serving an open-weight model yourself.
4. **Ownership**: buying hardware and running open-weight models on it.

The details per option (providers, prices, hardware) are on a separate page: [AI sourcing options](TODO).

Finally, I looked at what companies actually do, to see whether these trade-offs play out in practice.

## What happened

### No option wins

| Criterion | Subscription | Pay-per-use | Renting compute | Ownership |
|---|:---:|:---:|:---:|:---:|
| **Access** | ✓ | ✓ | ✗ | ✗ |
| **Economics** | ✓ | ~ | ~ | ~ |
| **Performance** | ✗ | ✓ | ✓ | ✓ |
| **Control** | ✗ | ~ | ✓ | ✓ |
| **Continuity** | ✗ | ~ | ~ | ✓ |
| **Effort** | ✓ | ✓ | ✗ | ✗ |

✓ = strong, ~ = depends on provider, volume or use case, ✗ = weak. This is my own judgement and a simplification.

The pattern is a trade-off. The options that are easy to start with are weak on control and continuity. The options that give you independence cost you access and effort.

### The weak spots are not theoretical

The ✗ marks for subscriptions and frontier APIs are not hypothetical. In the past year alone:

- **Access was cut for political reasons.** The US government imposed export controls on Fable 5 three days after launch, and Anthropic disabled it for all customers until the controls were lifted.[^fable-export] The Pentagon designated Anthropic a "supply chain risk", which an appeals court upheld.[^pentagon]
- **Terms changed.** Anthropic blocked the use of subscription logins in third-party tools such as OpenCode, so a subscription now only works inside Anthropic's own apps.[^oauth]
- **Behaviour changed.** Anthropic published postmortems for quality drops caused by infrastructure bugs and changed defaults,[^postmortems] independent daily benchmarks have flagged statistically significant drops,[^degradation] and some topics, such as dual-use biology, deliberately fall back to a less capable model.[^bio-safeguards]
- **Data residency is not sovereignty.** Microsoft France told the French Senate under oath that it cannot guarantee EU data is safe from US authorities, even when it is stored in the EU.[^cloud-act]

None of these make frontier providers bad choices. They are what renting looks like: the landlord can change the terms.

### Companies make opposite choices

If the trade-off is real, you would expect companies to land in different places. They do. The figures below are mostly reported by the companies themselves.

| Strategy | Examples |
|---|---|
| **All-in on a frontier provider** | [Cognizant](https://analyticsindiamag.com/ai-news/cognizant-commits-25000-professionals-to-deploy-claude-in-enterprises) is rolling out Claude across its workforce and set up a Claude group of more than 25,000 people. Deloitte is [training and certifying 15,000 professionals](https://intuitionlabs.ai/articles/claude-enterprise-deployment-training-guide-2026) on Claude. Engineers at Sierra [estimate](https://joseparreogarcia.substack.com/p/open-source-models-are-good-enough) they ship 3 to 20 times more productively with Claude Code. |
| **Both at once** | Uber reportedly [used up its 2026 AI coding budget in four months](https://joseparreogarcia.substack.com/p/open-source-models-are-good-enough) with roughly 5,000 engineers on Claude Code, while [moving other tasks to open models](https://blog.pragmaticengineer.com/the-pulse-tech-companies-move-to-open-ai-models/). |
| **Open by default, frontier as the exception** | [Revolut](https://ai.engineer/talks/Ok6QdZHCcI8-revolut-cut-ai-coding-costs-90-self) runs its coding agents on a self-hosted open-weight model and routes only vision tasks to frontier models, reporting about a tenth of the cost. Intercom [moved one high-volume summarisation job](https://consciousengines.com/blog/frontier-model-downshift-index) to a fine-tuned open model and kept its hardest customer-facing prompt on a frontier model. |
| **Own models** | Pinterest [reports](https://consciousengines.com/blog/frontier-model-downshift-index) a cost per transaction below 8% of comparable closed models. [C3 AI](https://c3.ai/blog/why-c3-ai-fine-tunes-its-own-models-even-with-frontier-ai-this-strong) fine-tunes its own models, even with strong frontier models available. |

Two things stand out. First, the companies that move away from frontier models rarely move away completely: they decide **per task**. Second, the all-in examples are often services firms, for whom betting on one provider is partly a commercial choice.

The aggregate data shows the same split, and it depends on what you measure. On Vercel's AI Gateway, open-weight models handled 56% of all tokens in August 2026, up from 7% in December 2025, but only 14% of the spend.[^vercel] Volume moves to open models; the money stays with the frontier. Measured by enterprise spend, the picture even looked the other way: in 2025, Menlo Ventures saw the open-model share of enterprise usage fall from 19% to 11%, with Anthropic at about 40% of enterprise LLM spend.[^menlo] And when open models do take over, the gain per task is smaller than the price per token suggests: Gartner notes that open models are more verbose and retry more often.[^gartner] AT&T reportedly cut its bill by 56% for a 2% drop in measured quality.[^att]

### My own portfolio

{% comment %} TODO(steff): revise rows, mark what is in use and what is planned. {% endcomment %}

| Layer | Provider / model | Used for | If it disappears |
|---|---|---|---|
| **Subscription** | Claude | Brainstorming and infrastructure coding | Another lab's plan |
| **Subscription** | Gemini | Video and image analysis | Another multimodal model, with some loss |
| **Pay-per-use** | Qwen via OVHcloud and Scaleway | All-round production workloads | The same model at another provider |
| **Pay-per-use** | DeepSeek via Scaleway | Cheap long-context coding and review | Slightly harder to replace |
| **Pay-per-use** | Mistral | Work where a European provider matters | An open-weight model elsewhere |
| **Ownership** | Small open-weight models | Learning and routine tasks | This *is* the fallback |

### What I couldn't settle

Two things remain open.

The first is practical: **on paper, I can't tell whether a cheaper model can do my work.** Whether an open-weight model on a European provider handles a clinical trials pipeline as well as Claude depends on success rates I haven't measured yet.

The second is a creeping feeling that **maybe none of this matters**, and we can simply stay with the frontier models. They keep getting better, and the effort of a nuanced setup is real. I also notice, in myself and in others, that working with a model as capable as Claude makes it tempting to stop understanding things yourself. Researchers at the MIT Media Lab call this "cognitive debt",[^cognitive-debt] and in a 2025 randomised trial by METR, experienced developers were 19% slower with AI tools while believing they were 20% faster.[^metr] I don't know yet whether this nuanced view on sourcing will prove worth it in hindsight.

## The lesson

There is more to choosing AI than "smarter is better". But the real issue turned out not to be cost. It's ownership.

**Relying fully on one frontier provider puts you in a rental relationship for what should be the core of your business.** Renting is not wrong. It is flexible, it is cheap right now, and for exploring it is the right choice. But rent builds no asset, and the landlord sets the price.

Two structural risks come with renting your core:

- **Platform risk.** A provider sees how its product is used, and can build what its customers build. That pattern is familiar from other platforms.
- **Value capture.** If a model does all of your work, why would a client pay you rather than the model? Your value has to sit in what the model doesn't have: knowledge of your domain and processes, verification a client can trust, and a stack that works within a client's constraints. {% comment %} TODO(steff): confirm. {% endcomment %}

Even the weights you depend on are not yours. Anthropic commits to preserving the weights of released models for the lifetime of the company,[^deprecation] which is preservation, not a guarantee that you can keep using them.

Investing in your own stack and knowledge is different from paying rent. It is a **capital investment**: it keeps its value whichever model wins. And the curve is steep: according to Epoch AI, the best open-weight models trail the closed frontier by only about four months.[^epoch] When today's best model is a commodity within months, the ability to switch is worth more than any single choice.

My working rule: **own the core, rent the edge.**

## What next

This post stayed qualitative. The obvious next question is when each strategy is actually the right one, and that needs numbers. In the next post I work out what an AI task really costs, including my own time, using the clinical trials session as an example.

And the open question I keep coming back to: **is all this nuance worth it, or will we look back and realise we should simply have stayed with the frontier models?**

{% comment %}
Footnote sources: items marked (notes) come from
_drafts/beyond-frontier-AI-part-I-research-notes.md (checked 2026-09-25 unless
noted); the rest were found by search on 2026-11-11 and have not been opened.
{% endcomment %}

[^fable-export]: (notes) [Implicator](https://www.implicator.ai/us-export-controls-force-anthropic-to-pull-fable-5-and-mythos-5-days-after-launch/)
[^pentagon]: (notes) [Axios](https://www.axios.com/2026/02/27/anthropic-pentagon-supply-chain-risk-claude) · [Mayer Brown](https://www.mayerbrown.com/en/insights/publications/2026/03/pentagon-designates-anthropic-a-supply-chain-risk-what-government-contractors-need-to-know) · [CNBC](https://www.cnbc.com/2026/09/25/pentagon-anthropic-ai-risk-appeals-court.html)
[^oauth]: (notes) [VentureBeat](https://venturebeat.com/technology/anthropic-cracks-down-on-unauthorized-claude-usage-by-third-party-harnesses) · [AlternativeTo](https://alternativeto.net/news/2026/2/anthropic-officially-bans-using-subscription-authentication-for-third-party-claude-use)
[^postmortems]: (notes) Anthropic postmortems: [Aug/Sep 2025](https://www.anthropic.com/engineering/a-postmortem-of-three-recent-issues) · [Mar-Apr 2026](https://www.anthropic.com/engineering/april-23-postmortem). These were bugs and configuration changes, not secret model swaps.
[^degradation]: (notes) [MarginLab tracker](https://marginlab.ai/trackers/claude-code)
[^bio-safeguards]: (notes) Opus 5.5 falls back to Opus 5 for dual-use biology; the fallback is shown with a notice. [Claude Help Center](https://support.claude.com/en/articles/16049681-why-claude-switched-models-in-your-conversation-with-opus-5-or-opus-5-5)
[^cloud-act]: (notes) [The Register](https://www.theregister.com/2025/07/25/microsoft_admits_it_cannot_guarantee/) · [Forbes](https://www.forbes.com/sites/emmawoollacott/2025/07/22/microsoft-cant-keep-eu-data-safe-from-us-authorities/)
[^vercel]: [Vercel AI Gateway production index, September 2026](https://vercel.com/blog/ai-gateway-production-index-september-2026) · [The New Stack](https://thenewstack.io/open-weight-anthropic-spend/)
[^menlo]: (notes) [Menlo Ventures, 2025: The State of Generative AI in the Enterprise](https://menlovc.com/perspective/2025-the-state-of-generative-ai-in-the-enterprise/)
[^gartner]: [The Register](https://www.theregister.com/ai-and-ml/2026/11/03/open-models-can-do-90-of-agentic-work-at-a-twentieth-of-the-cost-of-a-frontier-llm-gartner/5307132)
[^att]: Reported via [Open vs Frontier AI Models & the $100K Token Budget](https://medium.com/@huanzidage/open-models-vs-frontier-models-who-actually-wins-1a01bf0df160). TODO: find the original (Wall Street Journal interview).
[^cognitive-debt]: (notes) Kosmyna et al., MIT Media Lab, [arXiv 2506.08872](https://arxiv.org/abs/2506.08872)
[^metr]: [METR, Measuring the Impact of Early-2025 AI on Experienced Open-Source Developer Productivity](https://metr.org/blog/2025-07-10-early-2025-ai-experienced-os-dev-study/). Early-2025 tools; the authors warn against generalising.
[^deprecation]: (notes) [Anthropic deprecation commitments](https://www.anthropic.com/research/deprecation-commitments)
[^epoch]: (notes) [Epoch AI: open vs closed gap](https://epoch.ai/data-insights/open-closed-eci-gap)
