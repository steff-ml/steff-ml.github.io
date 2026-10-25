---
title: "Architecture of an independent AI stack"
date: 2026-10-18 09:00:00 +0200
categories: [build-log]
chapter: stack
part: 1
image: /assets/images/posts/independent-ai-stack-cartoon.jpg
image_alt: "Cartoon of a signalman in a signal box labelled Independent AI stack, switching a train of prompts and data onto one of three tracks: a large frontier models station with a lowered barrier, a regional station for European cloud providers, and a small self-hosted station next to a cabin with a vegetable garden."
concepts:
  - ai-gateway
excerpt: "Why I decided frontier models weren't enough and how switching became a one-line config change."
tags:
  - ai-engineering
  - litellm
  - opencode
---

## Why build an independent AI stack in the first place?
In order to build the automated AI tools envisioned in this product, I first needed to decide how I would incorporate the models in the tools.
Briefly, three major approaches were considered:
1. Get a frontier model provider subscription or an API key.
2. Get an API key from European cloud or AI providers that serve open-source models such as Mistral, Deepseek or Qwen3.8 27b.
3. Download open-source or open-weight models and serve them from my computer.

Each of these approaches came with its own failure risks.

**Frontier model providers**, such as Anthropic and Google give the best price/quality service. For only 20 euro/month, you get access to some of the best models out there (Opus 5.5, Gemini 4 Argon etc), nicely polished apps to perform the work and an active developer community and blogs that help you get the best out of AI.  My main concern with relying on these tools was operational variance and dependency. In the case of Anthropic, we have already seen export controls on Fable,[^fable-export] the introduction of deliberate model degradation in areas such as molecular biology[^bio-safeguards] and and independent daily benchmarks that have measured unexplained drops in quality.[^degradation] Frontier subscriptions are great for developing new products, but I wanted other options to guarantee operational stability.

**European cloud and AI providers** form a nice complement. European cloud providers like Scaleway and OVHcloud have started offering API endpoints to open-source models they are hosting on their servers.[^eu-endpoints] Mistral is even developing and hosting its own models. These API endpoints work exactly the same as the frontier endpoints. In exchange for some raw power, you gain access to models like Deepseek v4.1 Flash and Qwen3.8 27b that are often far cheaper for simple tasks than frontier ones. They also drastically reduce the risk of politically motivated export controls and regulatory requirements may make models like Mistral the only viable option. Like the frontier model providers, this option allows you to offload a ton of headaches related to serving, managing and (partially) securing these models.

**Self-hosted AI models** are the holy grail in terms of independence and control, but they come with the aforementioned headaches in serving, managing and securing the models. Since hardware is limited and expensive, I would only be able to run models capable of doing basic coding tasks and git management. Nevertheless, if a task can be completed by a self-hosted AI model, I can be confident that I can provide any service related to it in the future.

So I wanted to build an independent AI stack that allowed me to easily switch between these options.

## Components of an independent AI stack

The trick to building an independent AI stack is to introduce a decoupling layer between the model provider and your applications.
This decoupling layer, called an AI gateway, can then route calls related to a specific task to a model you preset in a config file.
Below are the components of my specific coding setup:



| Layer | What it does | What I use |
|---|---|---|
| Gateway | One interface for every tool; routes each call to a model by config | [LiteLLM, four routes: routine, commit, review, embed] |
| Models | Open-weight models, swappable behind the gateway | [OVHcloud, Scaleway] |
| Harness | Where the coding happens; house rules, skills, commands | [OpenCode] |
| Monitoring | A trace of every call: what it cost, what the model did and what we can learn from it | [Langfuse] |

<figure class="post-figure">
  <img src="{{ '/assets/images/posts/litellm-routing-config.png' | relative_url }}" alt="Excerpt of a LiteLLM configuration file with three routes, routine, commit and review, each pointing to an open-weight model at Scaleway or OVHcloud with its price per token">
  <figcaption>Example gateway configuration for model routing: each route names a task and points to a model. The next post goes deeper into this configuration.</figcaption>
</figure>


## Key learnings

1. AI can come from different sources, each with their own strengths and weaknesses.
2. The best way to work with AI models is to treat them as swappable commodities.
3. An independent AI stack allows me to quickly and efficiently swap between models.

## Next question

Now that the basic architecture of the independent AI stack is decided, I need to decide how to configure the different components. First up will be setting up the AI gateway to enable efficient model routing.

[^fable-export]: The US government imposed export controls on Fable 5 in June 2026, three days after launch; Anthropic disabled Fable 5 and Mythos 5 for all customers until the controls were lifted on 1 July. [Implicator](https://www.implicator.ai/us-export-controls-force-anthropic-to-pull-fable-5-and-mythos-5-days-after-launch/) · [The Next Web](https://thenextweb.com/news/anthropic-fable-5-export-controls-lifted)
[^bio-safeguards]: Anthropic screens requests with classifiers and deliberately falls back to a less capable model for some topics: Opus 5.5 falls back to Opus 5 for dual-use biology such as virology, toxicology and molecular design, and offensive cybersecurity requests can fall back to Opus 4.8. The fallback is shown with a notice. [Claude Help Center](https://support.claude.com/en/articles/16049681-why-claude-switched-models-in-your-conversation-with-opus-5-or-opus-5-5) · Earlier coverage of the broader biology safeguards on Fable 5: [Unite.AI](https://unite.ai/anthropic-retunes-fable-5s-biology-safeguards-cutting-blocked-queries-85)
[^degradation]: MarginLab runs Claude Code daily on a contamination-resistant subset of SWE-Bench-Pro and has flagged statistically significant drops, for Opus 4.5 in January 2026 and for Opus 4.7 before Opus 4.8 shipped. [MarginLab tracker](https://marginlab.ai/trackers/claude-code) · [MarginLab on Opus 4.7](https://marginlab.ai/blog/claude-code-degraded-before-opus-4-8/) · [Gigazine on Opus 4.5](https://gigazine.net/gsc_news/en/20260130-claude-code-opus-performance/). Anthropic's own postmortems attribute earlier quality drops to infrastructure bugs and changed defaults: [Aug/Sep 2025](https://www.anthropic.com/engineering/a-postmortem-of-three-recent-issues) · [Mar/Apr 2026](https://www.anthropic.com/engineering/april-23-postmortem)
[^eu-endpoints]: Both serve open-weight models through an OpenAI-compatible API. [Scaleway Generative APIs](https://www.scaleway.com/en/generative-apis/) · [OVHcloud AI Endpoints](https://docs.ovhcloud.com/en/guides/public-cloud/ai-machine-learning/ai-endpoints-catalog-api)
