---
title: "AI sourcing options"
permalink: /ai-sourcing-options/
author_profile: true
excerpt: "Five ways to get AI, from a monthly subscription to your own hardware, and what each one hands to the supplier."
---
{% comment %}
AI DRAFT, written by Claude, not reviewed. Delete this comment when you publish.

Built from your own text in _drafts/2026-10-24-model-sourcing.md (section
"AI sourcing options"), reorganised into the five options of the post and the
three layers (disqualifiers, supply risk, value). The economics stay implicit:
each option is described by what you buy, who else could sell it to you and
who carries which risk. No Kraljic or hold-up by name.

Changes to check:
- Mistral dropped from "subscriptions with API access": I could not confirm
  that a Mistral plan includes an API key.
- Subscription tiers: I changed "7-8 / 15-20 / 100 / 200 euro" to a range,
  because Gemini's plans are EUR 4.99 / 21.99 / 99.99 (checked 14 Nov 2026).
- Ollama Cloud (USD 20 and 100 plans with usage credits) checked on
  ollama.com/pricing. OpenCode Go (USD 10/month) only via its docs.
{% endcomment %}

There are five ways to get AI. They differ in two things: **what you actually buy**, and **whether anyone else could sell you the same thing**. Going down the list, you buy less from the supplier and do more yourself.

| Option | What you buy | Who else can sell you the same model | How you pay | Who runs it |
|---|---|---|---|---|
| **1. Subscription** | A finished product: apps plus model | Nobody (lab plans) or many (open-weight plans) | Flat monthly fee | The provider |
| **2. API, closed model** | Answers from one lab's model | Only the lab, and the clouds it licenses | Per token | The provider |
| **3. API, open model** | Answers from a model anyone may serve | Many providers | Per token | The provider |
| **4. Rented compute** | GPU time | Any cloud with GPUs | Per hour, used or not | You |
| **5. Own hardware** | The machine itself | Nobody needs to | Upfront, plus electricity | You |

Each option below is scored on the same three layers as in the post: **disqualifiers** (can I use it at all?), **supply risk** (how much power does the supplier have over me?) and **value** (what do I get for my money?).

## 1. Subscription

A monthly plan. You pay a flat fee and get the provider's apps: chat, coding agents, deep research and more.

- **From a lab:** [Claude](https://claude.com/pricing), [ChatGPT](https://chatgpt.com/pricing), [Gemini](https://gemini.google/subscriptions/). Prices run from about 5 to 20 euro a month, with power-user tiers around 100 to 200 euro.
- **For open-weight models, with an API key:** [Ollama Cloud](https://ollama.com/pricing), [OpenCode Go](https://opencode.ai/go). You get a real API key, so you can plug the models into your own tools.

**Why it is its own category:** you buy a product, not a model. The provider decides which model you get, how much you can use and what the apps can do. In return, the provider carries the risk that you use a lot.

- **Disqualifiers:** Consumer plans are often not meant for commercial use, so check the terms before building anything on them. Most lab providers are US-based.
- **Supply risk:** The highest of all options. The provider can change the model behind the app, throttle you with usage windows, change the terms or raise prices. Lab plans also lock you into their apps, and most give no API key, so you can't take the model elsewhere. Plans for open-weight models are better here: the same models are available elsewhere, so you can leave without rewriting your setup.
- **Value:** Best-in-class models and apps for almost no effort. Plans look cheap for what they offer, probably because investors are paying part of the bill today. You also pay for capacity you don't use.

**Use it when:** learning, research and keeping up with new tools. Not for production work that has to keep running.

## 2. API, closed model

You pay per token for a lab's own model and call it from your own software. This comes with commercial terms, sometimes a service level agreement and zero-data-retention options.

- **Directly from the lab:** [Anthropic](https://claude.com/pricing#api), [OpenAI](https://openai.com/api/pricing/), [Google](https://ai.google.dev/gemini-api/docs/pricing).
- **Through a hyperscaler:** [AWS Bedrock](https://aws.amazon.com/bedrock/pricing/), [Microsoft Foundry](https://azure.microsoft.com/en-us/products/ai-foundry/models), [Google Cloud](https://cloud.google.com/gemini-enterprise-agent-platform/generative-ai/pricing). One contract for many models, though new models can arrive later than at the lab.

**Why it is its own category:** you now own the application, but the model still has exactly one maker. A hyperscaler is another shop for the same product, not another product. If the lab changes the model, its price or who may use it, every shop follows.

- **Disqualifiers:** Commercial terms say what the provider may do with your data. Labs are mostly under US (or Chinese) law, and US-headquartered clouds remain exposed to US law even when your data stays in an EU region.
- **Supply risk:** Lower than a subscription, because terms and rate limits are written down. But access can still be cut, models are retired on the lab's schedule, and moving to another model means retesting your work. Buying through a hyperscaler adds a second lock-in: your cloud contract.
- **Value:** The best models with little effort: the provider runs everything. You pay only for what you use, which is cheap at low or spiky volume and expensive at high volume. Agentic work makes costs hard to predict, so budget limits and alerts are a must.

**Use it when:** you need the best model in your own software and the data may leave your hands.

## 3. API, open model

You pay per token for an open-weight model that a provider serves for you.

- **European providers:** [OVHcloud](https://www.ovhcloud.com/en/public-cloud/ai-endpoints/), [Scaleway](https://www.scaleway.com/en/generative-apis/), [Ionos](https://cloud.ionos.com/managed/ai-model-hub).
- **Mistral:** [Mistral](https://mistral.ai/pricing#api-pricing) sits in between. A single European supplier, with a model family that is partly open-weight.

**Why it is its own category:** on the invoice it looks like option 2. The difference is that the model has no single owner. Anyone may serve the same weights, so providers compete on price and service, and you can move to another provider, rented compute or your own machine without changing models.

- **Disqualifiers:** European providers offer EU jurisdiction and EU data residency.
- **Supply risk:** Low. If a provider raises prices, changes terms or disappears, the same model is available elsewhere. A downloaded model can't be taken back.
- **Value:** Open-weight models are usually a step behind the frontier. For routine work the gap often doesn't matter, and the price per token is often much lower. Effort is the same as option 2.

**Use it when:** the work doesn't need the very best model, or the data has to stay in Europe.

## 4. Rented compute

You rent GPUs from a cloud and run an open-weight model on them yourself. You choose the model, its version, the serving software and the region.

- **Large clouds:** [AWS](https://aws.amazon.com/ec2/instance-types/p5/), [Google Cloud](https://cloud.google.com/compute/docs/gpus), [Microsoft Azure](https://learn.microsoft.com/en-us/azure/virtual-machines/sizes/overview#gpu-accelerated), [Oracle Cloud](https://www.oracle.com/cloud/compute/gpu/).
- **European options:** [OVHcloud](https://www.ovhcloud.com/en/public-cloud/gpu/), [Scaleway](https://www.scaleway.com/en/gpu-instances/), [Hetzner](https://www.hetzner.com/dedicated-rootserver/matrix-gpu/), [Nebius](https://nebius.com/), [Verda](https://verda.com/).

**Why it is its own category:** you no longer buy answers, you buy capacity. The supplier only sells generic GPU time, which many clouds offer. In exchange, you take over the work, and you pay for the GPU whether it's busy or not.

- **Disqualifiers:** You choose where the data is processed and what is logged. That also makes the security and legal duties yours.
- **Supply risk:** Low on the model, since nobody can change or retire it for you. What remains is GPU supply, quotas and long commitments (often a year or more). The more you build on one cloud's own services (its Kubernetes, identity, networking, storage), the more you rebuild when you move. Containers and open tooling keep that small.
- **Value:** Stable, predictable performance on dedicated hardware, and models larger than you could buy yourself. At high, steady volume the cost per token can drop below per-token pricing; at low volume it's more expensive. Highest software effort so far: updates, serving software and monitoring are yours.

**Use it when:** volume is high and steady, the data is sensitive, or you need predictable performance from a model too large for your own hardware.

## 5. Own hardware

You buy machines and run open-weight models on them. Two choices define the setup.

**Inference software** turns downloaded weights into an API:
- [Ollama](https://ollama.com/) and [LM Studio](https://lmstudio.ai/): the easiest start on one machine.
- [llama.cpp](https://github.com/ggml-org/llama.cpp): runs on the widest range of hardware, including CPUs and Apple Silicon.
- [vLLM](https://github.com/vllm-project/vllm) and [SGLang](https://github.com/sgl-project/sglang): built for many requests at high throughput on GPUs.

**Hardware** is mostly a question of memory, because the model has to fit:
- **Dedicated GPUs** are fastest when the model fits in their memory: [NVIDIA GeForce](https://www.nvidia.com/en-eu/geforce/graphics-cards/), [NVIDIA workstation GPUs](https://www.nvidia.com/en-us/products/workstations/), [AMD Radeon AI PRO](https://www.amd.com/en/products/graphics/workstations/radeon-ai-pro.html).
- **Unified memory systems** fit larger models at lower speed: [NVIDIA DGX Spark](https://www.nvidia.com/en-us/products/workstations/dgx-spark/), [Apple Mac Studio](https://www.apple.com/mac-studio/), mini PCs with [AMD Ryzen AI Max+](https://www.amd.com/en/products/processors/laptop/ryzen/ai-300-series/amd-ryzen-ai-max-plus-395.html).

**Why it is its own category:** you buy an asset instead of a service. After the purchase, nobody can cut you off, change terms or raise prices. The cost moves from a monthly bill to an upfront investment, and every risk the supplier used to carry is now yours.

- **Disqualifiers:** The strongest data guarantee: nothing leaves the building, so the jurisdiction question is easy. Full control also means full legal responsibility.
- **Supply risk:** The lowest towards AI providers. A downloaded model keeps working even if its maker disappears. New dependencies appear instead: a broken machine takes you offline with no failover, scaling means buying hardware (whose prices swing, as memory prices recently showed) and you depend on the people who maintain it.
- **Value:** The weakest models of all options, limited to what fits in your memory and slow to upgrade. Highest effort: hardware, cooling and software are all yours. After purchase, a token costs little more than electricity, so for steady work the cost can end up lowest, and the hardware keeps some resale value.

**Use it when:** the data is too sensitive for anyone else, small models can do routine work, or essential operations must keep running when every outside source fails. It's the generator, not the power grid.

## How the options compare

| | Subscription | API, closed model | API, open model | Rented compute | Own hardware |
|---|:---:|:---:|:---:|:---:|:---:|
| ***Disqualifiers*** | | | | | |
| Data residency / GDPR | ✗ | ~ | ✓ | ✓ | ✓ |
| Export controls / access cut off | ✗ | ✗ | ✓ | ✓ | ✓ |
| Provider's usage policy | ✗ | ✗ | ~ | ✓ | ✓ |
| ***Supply risk*** | | | | | |
| Price and rate-limit power | ✗ | ✗ | ✓ | ✓ | ✓ |
| Silent quality changes | ✗ | ~ | ✓ | ✓ | ✓ |
| Outages / model retirement | ✗ | ✗ | ~ | ~ | ✓ |
| Lock-in | ✗ | ~ | ✓ | ✓ | ~ |
| ***Value*** | | | | | |
| Capability | ✓ | ✓ | ~ | ~ | ✗ |
| Effort | ✓ | ✓ | ✓ | ✗ | ✗ |
| Cost | ✓ | ~ | ✓ | ~ | ~ |

✓ = strong, ~ = depends on provider, volume or use case, ✗ = weak. Personal judgement and a simplification.

No option wins on every layer. Going down the list, the supplier has less and less power over you: first you can take your model elsewhere, then you only rent generic hardware, then you own it. What you pay for that is capability, effort and money spent upfront. That trade-off is why a mix of options makes more sense than any single one.

