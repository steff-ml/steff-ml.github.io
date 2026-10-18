---
title: "Beyond frontier AI part I: Building a flexible, open-source AI infrastructure"
date: 2026-09-25 09:30:00 +0200
categories: [engineering]
tags:
  - ai
  - data-products
  - automation
excerpt: "How I use AI to automate and derisk the creation of data products."
---

# Introduction
- I love using frontier AI models and am a frequent user of Claude models.
  - The models are really powerful, especially on complex tasks with vague descriptions
  - Claude documentation is well maintained and the engineers are very generous with good blogs on best practices.
  - It has expanded access to people without a technical background by coming with several consumer friendly products like chat, Cowork, Claude Code with basic web hosting etc. Anyone can build applications with these tools now.
  - At 20 euro per month for the Pro account, Claude offers amazing bang for your buck.

- The engineer and tech enthusiast in me is super excited for this product.
- The entrepreneur in me sees several operational problems:
  - Operational dependency: What happens if I use it in mission-critical processes and practical access gets denied. This can happen for a variety of reasons:
    - Export controls: The US government bans Anthropic from working outside of the US.
    - Financial collapse: What happens to my mission-critical process if Anthropic goes bankrupt or nobody can sustain the cost of frontier offerings?
    - Price hikes: The whole sector could decide to increase the price of frontier access beyond my purchasing power.
    -
  - Operational variance: 
    - Performance degradation: The model behind the API is changed without my knowledge.
    - Guardrails: Anthropic adds guardrails to its product that make it unuseable for my mission-critical application.


  - Moat loss: 
    - Data leakage: Anthropic uses my data or gathers my usage data for their own competing products. 
    - Cognitive debt: I rely so much on Claude that I don't even know how my mission-critical process works  
  - Privacy: For certain sensitive workloads, it may not even be legal 
- None of these risks are theoretical. They have all either already happened, have been alleged to have happened or are considered a realistic possibility by experts.
- For this reason, I started exploring what could be done.

# Compound AI systems are the perfect complement to frontier intelligence
- Compound AI systems are a combination several tools, including code, vector databases, skills, harnesses and also AI models to perform specific tasks that you want. The core idea is that you can get frontier level results with far simpler models if you carefully optimize the tools and information they have available.
- The engineering of your own compound AI systems addresses the major issues related with using frontier intelligence in operations:
  - Operational dependency: Compound AI systems can be built with the many open-source tools and models currently available. This gives you much more options, ranging from the APIs of other AI labs, the APIs of cloud vendors and even invest in local hardware. Many of these are already competitive on most tasks businesses are dependent on, a fact underlined by business leaders saying that previous model iterations already cover their needs.
  - Operational variance: The flexibility of compound AI systems allow you to use the right tool for the right job. Most tasks you give to an AI consist of subtasks that do not require an AI at all. Deterministic, old-school testable code is often strictly superior in those cases and compound AI systems thinking gives you that discipline to finding the right tool for the right job.
  - Moat loss: AI tasks can be routed towards more stable partners or even your own hardware if you desire. Compound AI systems expand your options.
  - Privacy: Some workloads legally require local hosting for compliance.

# The components of compound AI systems
## AI gateways
- AI gateways are pieces of software that allow you to flexibly swap between different AI endpoints.
- They provide a unified interface to wire applications like coding harnesses, chat interfaces or even general programs too.
- In the backend, they route your queries to different API's based on a single line in your configuration.
- They also come with cost and budget controls allowing you to put caps on your spend.
- I personally run LiteLLM locally, but other options are available. Databricks offers Unity Gateway for this task on their platform.
## AI models
- The mindset shift to undergo is that GenAI models are a commodity in 2026 and you have plenty of options you can get them from.
  - Frontier API's: You don't have to stick to any one of the major AI labs. Anthropic, OpenAI, Google, xAI, Deepseek, Alibaba, ... all offer API's where you can buy access to their models on the meter. Do note that the pricing of these API's are way less attractive than their subscription counterparts. I personally never use the API's.
  - Cloud providers: An overlooked option, but a personal favorite. Major cloud providers offer metered access to open-source models that they host on their servers. This includes Databricks via their Foundational Model API's, but increasingly also local options like Scaleway, OVHCloud etc. Mistral is also worth a shout out in that regard, since they are a fully European AI lab offering access to their own models. These options can drastically reduce regulatory operational risks.
  - Self hosting: Open source tools like Ollama, lama.cpp and vLLM allow you to run open-source models yourself on hardware that you own

- The API's and self hosting options usually work with the OpenAI API format, turning switching between these backend options in a few lines of config changes.


## AI application layers
- Coding harnesses like Claude Code provide a new way to interact with LLM's
- Optimized for coding they allow you to configure hooks, skills, etc to make coding with LLM's more reliable
- Many popular harnesses exist such as OpenCode, Pi, Deepseek harness and Qwen harness to make the most of your AI as a coding support tool.
- I personally use OpenCode, but intend to run a few experiments to see what is optimal.
- Chat interfaces like Claude.ai also have open-source or free competitors on the market like OpenWebUI or LMstudio.

## AI performance monitoring
- Like with conventional code, you would like to be able to quantify AI performance.
- Monitoring tools like Langfuse use an OpenTelemetry standard to track what your AI model has been reasoning about.
- Evals are the AI equivalent of automated testing and allow you to track model performance on your tasks.
- This information can be very helpful in assessing inefficiencies in your system or protecting you against model performance degradation.


# Conclusion
I still use frontier models all the time, but they are fighting to earn their place. Claude is still the preferred option for non mission-critical things (like building a website) or for building and testing new workflows (more on that in part II). If you want to inspect my compound AI stack please visit <repo>