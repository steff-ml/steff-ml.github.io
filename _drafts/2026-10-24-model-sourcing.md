---
title: "AI model sourcing: Independence requires a portfolio of AI model delivery mechanisms."
date: 2026-10-24 09:00:00 +0200
categories: [build-log]
chapter: stack
part: 1
image: /assets/images/posts/independent-ai-stack-cartoon.jpg
image_alt: "Cartoon of a signalman in a signal box labelled Independent AI stack, switching a train of prompts and data onto one of three tracks: a large frontier models station with a lowered barrier, a regional station for European cloud providers, and a small self-hosted station next to a cabin with a vegetable garden."
concepts:
  - ai-gateway
excerpt: "Supplying your business with AI that fits your needs"
tags:
  - ai-engineering
  - litellm
  - opencode
---

## Introduction
In order to build the independent AI stack that will power my goals (automated-data-discovery, automated-pretotyping, automated-prototyping, ...), I first needed to get access to suitable AI models. When thinking this through, I quickly realized "suitable" could mean a lot of things depending on the use case and I wanted my AI stack to be able to handle different use cases. For example, automated-data-discovery involves reliably searching the web for data, interpreting papers and the pictures contained within and other unstructured sources and more, while automated-prototyping requires good software engineering capabilities on cloud providers. Additionally, there is now a wide range of AI delivery methods to choose from ranging from subscription models, pay-per-use and more, that also have their own pros and cons depending on the use case.

The internet is currently filled with detailed specs on which model performs better on each task at which hardware level, but comparatively little guidance on how to pick the best AI models from a broader economic perspective. I needed an economic perspective on AI sourcing. 


## AI as a commodity: defining criteria
Two metaphors currently dominate how AI is viewed from a business perspective. 

The first one sees AI (and agentic AI in particular) as a very smart person capable of doing valuable tasks for the business. While this metaphor is helpful for finding ways to integrate AI into your business, it encourages the naive interpretation that a smarter coworker automatically means a better coworker. As we will see, nothing could be further from the truth: the smartest models are usually the most expensive, the hardest to replace and the most exposed to a single vendor's decisions.

The second metaphor presents AI as a commodity that businesses need to sustain consistent operations, similar to electricity or water. Many businesses not only draw electricity from the net, but have a wide range of solutions like batteries, solar panels and emergency generators to guarantee operational stability. AI supported businesses face the same challenge: What happens if my access to AI models is cut off or made illegal? 


This framing helped me define the following criteria:
| Criterion | Core question |
|---|---|
| **Access** | Which models do I have access to and how soon after release? |
| **Economics** | What is the price, who carries what risk and what do I pay for?|
| **Performance** | Is it available, fast, consistent...? |
| **Control** | Where does my data go, under what jurisdiction, what do the licence and terms-of-service allow me to do? |
| **Continuity** | Am I locked in with a vendor, can I switch cheaply, how reliable is the vendor,...? |
| **Effort** | Do I have to build and maintain solutions myself? |

This set of criteria here will form the lens from which we judge different AI sourcing options.


## AI sourcing options

### 1. AI-as-a-subscription
The first and for many people only way of procuring AI models is to get a **consumer plan** via one of the major labs ([Anthropic](https://claude.com/pricing), [Google](https://gemini.google/subscriptions), [OpenAI](https://chatgpt.com/pricing),  [Perplexity](https://www.perplexity.ai/pro)). As of October 2026, the industry has aligned on a few tiers (Free, 7-8 euro per month, 15-20 euro per month, 100 euro per month and 200 euro per month). 

**Strengths**:
- **Access**: Consumers get access to best-in-class models AND applications such as deep research, coding agents, design tools and more. Note that not all models have the same capabilities. For example, Claude models are often best-in-class in coding, but lack the video understanding capabilities Gemini has.
- **Economics**: Consumer plans charge a flat rate, transferring overuse risk to the provider. However, this also means that consumers pay for underused capacity. Currently though, consumer plans appear abnormally cheap, most likely thanks to heavy venture capital subsidies, so for moderate, steady use they are economically a no brainer.
- **Effort**: None. You sign up and get immediate access to the latest models and tools.


**Weaknesses**:
- **Performance**:  Consumers have no control over which model is being served to them. Providers can change the model they are serving, throttle access and implement 5-hour reset windows, which preclude any serious production use cases.
- **Control**: Consumer plans are not meant for commercial activities, so check terms of service before building production use cases on them. Most consumer plans also do not offer API credits, making it  difficult to use the models outside the original tools. 
- **Continuity**: Consumer plans enable consumers with their apps, but also lock them into their platform with them. Heavy subsidies for consumer plans also mean they are not priced sustainably, so sudden price changes, withdrawn tiers or even provider bankruptcy are real possibilities. Most providers are also US-based, so access can be restricted for political reasons.


A lesser known addition to the subscription models are **plans with API access**. Offered by a whole host of institutions ([Ollama Cloud](https://ollama.com/pricing), [OpenCode Go](https://opencode.ai/go), [Mistral](https://mistral.ai/pricing), ...), these models offer access to open-weight models via a real API key at various price points. This gives a ton of complementary advantages:
- **Access**: API based plans allow consumers to plug models into their own applications. 
- **Continuity**: API subscription plans expand the number of possible providers and use open-weight models, which can later be self hosted or moved to a pay-per-use scheme without rewriting your setup.  
- **Economics**: In some cases, unused API credits don't roll over, so you do pay for idle capacity.

However, these API subscription plans still often implement 5 hour resets, performance caps and many of them are still based outside the EU.

**Verdict**: Subscriptions are the best way to learn about AI and keep up with the newest tools. They are good for research and non-commercial use, but too unreliable for scalable production use cases. Plans with API access are a natural first step to start working with APIs, before you move to full pay-per-use.


### 2. Pay-per-use
The second method of procuring AI is via pay-per-use. In this plan, the model provider gives you access to an API that consumer programs can call at any time, paying only the tokens that were consumed. Unlike subscription plans, pay-per-use plans are made with production use cases in mind and come with contracts, SLAs (promised performance) and sometimes zero-data-retention options. 

**Strengths**:
- **Access**: API access means that these allow you to integrate AI into your own applications.
- **Economics**: You pay only for what you use, so no paying for idle capacity. For small and spiky volumes, APIs can be cheaper than subscriptions, even though they are less heavily subsidized. 
- **Performance**: SLAs and rate limits legally constrain what the model provider can do in terms of degrading model performance.
- **Effort**: The model provider still runs the infrastructure. Consumers only need to integrate the API.
- **Control**: Data retention and training terms are explicit about what a model provider can do with your data. However, legal constraints are not the same as explicit control.


**Weaknesses**:
- **Economics**: At large volumes, API costs can be far higher than the equivalent subscription. Paying for what you use also means that overuse risk now lies with the consumer. Especially for agentic workloads, it can be hard to predict how much a task will cost. Budget limits and alerts are a must when doing agentic work on APIs.

These are general thoughts on APIs versus subscription plans. Access, control and continuity concerns also depend heavily on who is providing API access.
Here are some options:

| Provider type | Examples | Access | Control | Continuity |
|---|---|---|---|---|
| **Model labs** | [OpenAI](https://openai.com/api/pricing/), [Anthropic](https://claude.com/pricing#api), [Google](https://ai.google.dev/gemini-api/docs/pricing), [DeepSeek](https://api-docs.deepseek.com/quick_start/pricing) | Their latest and best models | Mostly US or Chinese jurisdiction | Training costs are high and profitability is unclear, so pricing, terms and in some cases survival are uncertain. |
| **Hyperscalers** | [AWS Bedrock](https://aws.amazon.com/bedrock/pricing/), [Gemini Agent Platform](https://cloud.google.com/gemini-enterprise-agent-platform/generative-ai/pricing), [Microsoft](https://azure.microsoft.com/en-us/products/ai-foundry/models), [Oracle](https://www.oracle.com/artificial-intelligence/generative-ai/generative-ai-service/) | Many providers' models behind one contract, though new models are not immediately available | One contract eases access management, logging and compliance, but US-headquartered providers remain exposed to US law even when data is retained in EU regions | Single contract deepens lock-in with your cloud provider |
| **European serverless inference** | [OVHcloud](https://www.ovhcloud.com/en/public-cloud/ai-endpoints/), [Scaleway](https://www.scaleway.com/en/generative-apis/), [Ionos](https://cloud.ionos.com/managed/ai-model-hub) | Open-weight models, usually a step behind the frontier | EU jurisdiction and EU data residency | Open-weight models are easy to replace by other providers, self hosting or rented options. |
| **Mistral** | [Mistral](https://mistral.ai/pricing#api-pricing) | Strong models, though not always matching the top US labs | European provider | A single supplier, but the model family is partly open-weight and access is less likely to be revoked for political reasons. |

**Verdict**: Pay-per-use is a viable option for production use cases. They come with commercial terms and no unused cost. Controlling spend is tricky though, especially for agentic workflows and actual viability of a pay-per-use option depends on both the task that needs to be done (sensitive or not) and the jurisdiction of the model provider. While frontier models remain difficult to replace, open-weight models are offered by many more organizations, making switching between them cheaper and more redundant.

### 3. Renting compute
With renting compute plans, consumers rent cloud GPU instances and run their models on top. They choose the model (weights, quantization, ...), what serving software is used and where they run it.

**Strengths**:

- **Performance**: You have full control over the model, including the version, quantization and more. The GPU compute capacity should also be dedicated and stable, so latency and throughput should be completely predictable barring major outages.
- **Control**: Data stays inside the rented environment, meaning consumers can control in which region and under what jurisdiction their data is processed. Retention and logging are set by the consumers.
- **Economics**: Consumers pay for capacity, not for tokens. At high, steady volume the cost per token can fall below pay-per-use. Compute renting also doesn't require capital investment, making it viable to run larger models than your own hardware allows. 
- **Continuity**: Many cloud platforms rent out GPU's, both inside and outside the EU, making switching possible. Major cloud vendors include [AWS](https://aws.amazon.com/ec2/instance-types/p5/), [Google Cloud](https://cloud.google.com/compute/docs/gpus), [Microsoft Azure](https://learn.microsoft.com/en-us/azure/virtual-machines/sizes/overview#gpu-accelerated) and [Oracle Cloud](https://www.oracle.com/cloud/compute/gpu/), while European options include [OVHcloud](https://www.ovhcloud.com/en/public-cloud/gpu/), [Scaleway](https://www.scaleway.com/en/gpu-instances/), [Hetzner](https://www.hetzner.com/dedicated-rootserver/matrix-gpu/), [Nebius](https://nebius.com/) and [Verda](https://verda.com/). 

**Weaknesses**:
- **Access**: This approach is only possible for open-weight models.
- **Control**: More control can also mean more exposure, since certain security and legal requirements are shifted to the consumer. 
- **Economics**: Consumers pay for capacity whether it's used or not. At low or spiky volume, pay-per-use is cheaper.
- **Effort**: Consumers are responsible for many parts of the infrastructure, including managing updates and setting up the serving software. Economies of scale can make running high-end open-weight models viable for model providers in a way that never makes sense for individual users.
- **Continuity**: GPU supply and quotas can limit consumers and long commitments (often a year or more) lock consumers in. Deploying on a cloud environment also creates rework risk: the more consumers build on a provider's own services (managed Kubernetes variants, identity, networking, storage), the more they have to rebuild when they move. Packaging deployments in containers and using open tooling minimizes that rework, but it can still be significant. 


**Verdict**: Compute renting makes sense when volume is high and steady, when data is sensitive or when controlled performance is a strong requirement and when the models are too large to run on your own equipment. 

### 4. Ownership
With ownership, consumers buy their own hardware and run open-weight models there. They pay for the upfront capital investment and the electricity used for the machine, but otherwise pay nothing. Two choices define the setup: the **inference software** that serves the model and the **hardware** it runs on.

Inference software turns downloaded model weights into an API that other applications can call:
- [Ollama](https://ollama.com/) and [LM Studio](https://lmstudio.ai/) are the easiest way to get started on a single machine.
- [llama.cpp](https://github.com/ggml-org/llama.cpp) runs on the widest range of hardware, including CPUs and Apple Silicon.
- [vLLM](https://github.com/vllm-project/vllm) and [SGLang](https://github.com/sgl-project/sglang) are built to serve many requests at high throughput on GPUs.

Hardware is mostly a question of memory, because the model weights have to fit. Memory comes in two forms: VRAM on a dedicated GPU, or unified memory shared between CPU and GPU. Dedicated GPUs are fastest when the model fits in their VRAM, while unified memory systems fit larger models, typically at lower speed:
- **Dedicated GPUs**: [NVIDIA GeForce](https://www.nvidia.com/en-eu/geforce/graphics-cards/), [NVIDIA workstation GPUs](https://www.nvidia.com/en-us/products/workstations/) and [AMD Radeon AI PRO](https://www.amd.com/en/products/graphics/workstations/radeon-ai-pro.html).
- **Unified memory systems**: [NVIDIA DGX Spark](https://www.nvidia.com/en-us/products/workstations/dgx-spark/), [Apple Mac Studio](https://www.apple.com/mac-studio/) and mini PCs built on [AMD Ryzen AI Max+](https://www.amd.com/en/products/processors/laptop/ryzen/ai-300-series/amd-ryzen-ai-max-plus-395.html).


**Strengths**:
- **Control**: Data never leaves the premises, the strongest guarantee that third parties don't see your data. Since data does not leave the building, the jurisdiction question becomes easy as well.
- **Continuity**: Consumers own the hardware, so nobody can cut off access, change terms or raise prices. Downloaded open-weight models keep working indefinitely, even if the provider that released them disappears. This is the closest equivalent to having your own generator in the electricity metaphor.
- **Performance**: Consumers have full control over both the deployed model and the underlying hardware. No shared queues, no network latency and no model version changes.
- **Economics**: After the hardware purchase, the marginal cost of a token is mostly the electricity needed to run the machine. For high, steady workloads the cost per token can end up below any other option and the hardware keeps resale or reuse value.

**Weaknesses**:
- **Control**: Maximum control means maximum legal exposure.
- **Economics**: Ownership requires upfront capital investment. Furthermore, hardware loses value, and capacity sits idle when demand is low. Prices for GPUs and memory can swing as evidenced by the recent increases in RAM and VRAM costs.
- **Access**: The most restricted of all options. Models are limited to what fits in the memory you own, usually smaller or heavily quantized open-weight models. These are heavily behind the frontier and it is not quick and easy to upgrade like with rented compute.
- **Effort**: Like rented compute, consumers need to manage all the software management tasks. Unlike rented compute, ownership also requires hardware management e.g. setting up GPU's, providing cooling and more. The highest effort option.
- **Continuity**: Ownership removes dependence on a provider but adds new dependencies. A hardware failure takes you offline with no automatic failover. Scaling means buying more machines, creating a dependency on hardware providers. The effort needed to maintain these systems also creates a dependency on the engineers to set up and maintain them.


**Verdict**: Ownership should not be the primary source of AI for most operations. It should be limited to sensitive workloads, small models doing routine work and keeping essential operations running when every external source fails. In the electricity metaphor, it's the battery or generator: it keeps the lights on.

### Summary
Here is how the four options score on the six criteria. This is my own judgement and a simplification, since results vary by provider and use case.

| Criterion | Subscription | Pay-per-use | Renting compute | Ownership |
|---|:---:|:---:|:---:|:---:|
| **Access** | ✓ | ✓ | ✗ | ✗ |
| **Economics** | ✓ | ~ | ~ | ~ |
| **Performance** | ✗ | ✓ | ✓ | ✓ |
| **Control** | ✗ | ~ | ✓ | ✓ |
| **Continuity** | ✗ | ~ | ~ | ✓ |
| **Effort** | ✓ | ✓ | ✗ | ✗ |

✓ = strong, ~ = depends on the provider, volume or use case, ✗ = weak.

No option wins every row. The options that are easy to start with (subscription, pay-per-use) are weakest on control and continuity, and the options that give you independence (renting, ownership) cost you access and effort. That trade-off is why a portfolio makes sense.

## Portfolio of AI delivery mechanisms and my own stack
From the above analysis of different AI sourcing options, it is clear that there is no one-size-fits-all solution.
Each of the methods has pros and cons and will be the clearly superior solution for different use cases. 
From this, it follows that an independent AI stack that accommodates a wide range of use cases should use multiple of these sources.
I thought it would be helpful to share the models that I use, what I use them for and why:

| Layer | Provider / model | Used for | Why | If it disappears |
|---|---|---|---|---|
| **Subscription** | Claude (20 euro per month) | Brainstorming and infrastructure coding | **Access**: top-tier models and the most practical coding tools (Claude Code, Design) | Another lab's coding plan, since nothing in production depends on it. ChatGPT is close enough to replace most of this functionality |
| **Subscription** | Gemini Pro (20 euro per month) | Video and image analysis and generation | **Access**: multimodal capabilities I can't get from any other vendor, plus extra storage and cloud credits | Another multimodal model, with some loss of capability |
| **Pay-per-use** | Qwen3.8 27B via OVHcloud and Scaleway | Good all-rounder for production workloads | **Continuity**: open-weight and offered by several vendors. **Control**: EU provider | The same model at another provider |
| **Pay-per-use** | DeepSeek 4.1 Flash via Scaleway | Cheap option with long context window for complex coding tasks and review | **Continuity**: open-weight, but offered by fewer vendors so no critical uses. **Control**: EU provider | The same model at another provider, but slightly harder to replace. |
| **Pay-per-use** | Mistral | Workloads where a European provider matters | **Control**: preferred European provider, which may be important for future customers | Switch to an open-weight model at another provider |
| **Ownership** | Small open-weight models on my own machine, such as Qwen3.5 9b | Learning and small routine tasks like commit message writing | **Continuity**: keeps working when every external source fails | This is my fallback |

Two principles govern this table:
- **Subscriptions are for exploration, pay-per-use is for production.** I do not use nearly all the tokens of either subscription. I choose to pay too much to learn from two top-tier providers. The analysis above is good to keep in mind, but you still need to decide for yourself whether slight economic inefficiencies are worth other rewards.
- **In production I value redundancy over quality.** I don't use frontier APIs, because if they fail I may not be able to replace that functionality. I prefer models offered by several vendors, like the Qwen line. Mistral is my exception.


**A note on ownership**: At current prices, it is usually not worth it to upgrade your computer or rent compute. Instead, I am more inclined to adapt the models to my hardware specs. Given current inference prices, it also makes no economic sense to run some of these models myself. Running models myself is just the fastest way to learn how they work and to get the most out of my other options. In a future blog, I go into detail about how I run models on my own machine. Since storage is cheap, I will start a personal collection of model weights I deem useful for local use or even rented compute.



## Conclusion
I hope this blog has convinced you that there is more to model sourcing than smarter AI = better AI.
Achieving an independent AI stack does not require you to go to your local hardware store and buy up all their equipment. Instead, I think it requires giving yourself a portfolio of sourcing options and a clear understanding of when to use what. However, this only becomes practical if switching between options is low-cost itself. For that reason, I will explore in the next blog how to set up the infrastructure to achieve that and more.



