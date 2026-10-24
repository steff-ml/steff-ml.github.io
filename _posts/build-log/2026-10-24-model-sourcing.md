---
title: "AI model sourcing: Independence requires a portfolio of AI model delivery mechnisms."
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
In order to build the independent AI stack that will power my goals (automated-data-discovery, automated-pretotyping, automated-prototyping, ...) , I first needed to get access to suitable AI models. When thinking this throughs, I quickly realized "suitable" could mean a lot of things depending on the use case and I wanted my AI stack to be able to handle different use cases. For example, automated-data-discovery involves reliably searching the web for data, interpreting papers and the pictures contained within and other unstructured sources and more, while automated-prototyping requires good software engineering capabilities on cloud providers. Additionally, there is now a wide range of AI delivery methods to choose from ranging from subscription models, pay-per-use and more, that also have their own pros and cons depending on the use case.

The internet is currently filled with detailed specs on which model performs better on each task at which hardware level, but comparatively little guidance on how to pick the best AI models from a broader economic perspective. I needed an economic perspective on AI sourcing. 


## AI as a commodity: defining criteria
Two metaphors currently dominate how AI is viewed from a business perspective. 

The first one sees AI (and agentic AI in particular) as a very smart person capable of doing valuable tasks for the business. While this metaphor is helpful for finding ways to integrate AI into your business, it encourages the naieve interpretation that a smarter coworker automatically means a better coworker. As we will see, nothing could be further from the truth.

The second metaphor presents AI as a commodity that businesses need to sustain consistent operations, similar to electricity or water. Many businesses not only draw electricity from the net, but have a wide range of solutions like batteries, solar panels and emergency generators to guarantee operational stability. AI supported businesses face the same challenge: What happens if my access to AI models is cut off or made illegal? 

This framing helped me define the following criteria:
- **Capability**: Can the models in my stack handle the tasks my use cases require of them?
- **Reliability**: Is the performance of models reliable?
- **Cost**: How much do the models cost at normal use?
- **Infrastructure setup**: Do I get a timely response from the model? Is it hard to maintain this solution?
- **Cost**: What do the models cost?
  - **Cost structure**: Are we charged a flat or variable rate? Who pays if my use case suddenly consumes more tokens than expected?
  - **Unused capacity**: Who pays when not all allotted tokens are used?
  - **Price and lock-in**: What happens if my provider suddenly decides to charge more?
  - **Supply chain risk**: What if my provider is no longer capable of providing me with the model? Can I cheaply switch?
- **Legal**: Am I allowed to use a particular model for a particular use case? 
  - **Model license**: Does the creator allow me to use this model for commercial activities if applicable?
  - **Data residency**: Am I allowed to send it sensitive data e.g. patient information?

The AI-is-a-commodity metaphor invites much richer questions about where to source AI models. The set of criteria here will form the lens from which we judge different AI sourcing options.


##  AI sourcing options

### 1. AI-as-a-subscription
The first and for many people only way of procuring AI models is to get a **consumer plan** via one of the major labs (Anthropic, Google, OpenAI, Mistral, Perplexity). The industry has aligned on a few tiers (Free, 7-8 euro per month, 15-20 euro per month, 100 euro per month and 200 euro per month). Consumer plans are attractive for many reasons:
- **Capability**: Consumers get access to best-in-class models AND applications such as deep research, coding agents, design tools and more. Note that not all models have the same capabilities. For example, Claude models are often best-in-class in coding, but lack the video understanding capabilities Gemini has.
- **Cost**: The subscriptions charge a flat rate, transferring overuse risk to the provider. They are currently far cheaper per token than other AI delivery methods, due to heavy venture capital subsidies.
- **Infrastructure setup**: Consumer plans require no infrastructure setup.

Consumer plans are great for people to get started with AI, but have a few disadvantages when use cases become more complicated:
- **Model licence**: Consumer plans are not meant for commercial activities. Note that most consumer plans do not offer API credits, making it very difficult to use the models outside the original tools.
- **Price and lock-in**: The applications that make life easy inside the consumer plan, also lock you into it and make you vulnerable to price changes.
- **Supply chain risk**: The heavy subsidies used to keep subscription plan costs down also creates a supply chain risk, as these plans are inherently not profitable.
- **Reliability**: Consumers have no control over which model is being served to them. Model degradations are openly admitted to when users trigger guardrails (i.e. biology questions on Anthropic, cybersecurity questions). To curb the costs of these plans, many providers work with 5-hour reset windows, which preclude any serious production use cases.
- **Legal**: Many of the consumer plan providers are US based. If you are not residing there, access can be taken at any point.

A lesser known addition to the subscription models are **API subscription models**. Offered by a whole host of institutions (Ollama Cloud, OpenCode Go, Alibaba Cloud, GLM coding plan, ...), these models offer access to open-source models via a real API key at various price points. This gives a ton of complementary advantages:
- **Capability**: API based plans open up many new integrations into your own applications not possible with the consumer plans.
- **Supply chain risk**: The multitude of providers sharply decreases the risk of failure of one. 
- **Price and lock-in**: The use of open-source models gives you more options to later switch to one of the other options below.

However, these API subscription models still do not address the legal, reliability or model license concerns of consumer plans.

Taken together, subscription plans are interesting mainly for consumers and small non-commercial applications. They are great for research and staying up to date with the newest apps, but are not suitable for heavy duty production use cases.

### 2. Pay-per-use
The second method of procuring AI is via pay-per-use. In these plans, model providers offer API access that consumers can call at any time and pay only for the number of tokens they consumed. This has the following advantages:
- **Cost**: Pay-per-use is usually cheaper than subscription models if volume is small and spiky, because you only pay what you use. It is more expensive at large volumes. Disadvantage is that overuse risk is shifted to you, which can be unpredictable with agentic use cases.
- **Model licence**: Pay-per-use is meant for commercial activities and comes with contracts, SLA's and even zero data retention options.
-**Infrastructure setup**: Like with consumer plans, model providers take full ownership of the infrastructure.

Furthermore, many organizations offer pay-per-use options, each with unique advantages:
- **API access from model providers (OpenAI, Anthropic, Google, Deepseek, ...)** give access to their latest and best models, but are often based in US and China, making it fall under their data protection jurisdictions. These companies are also not very profitable due to high training costs, reintroducing supply chain risk.
- **API access via hyperscalers (Microsoft, Oracle, AWS bedrock, Google Vertex AI, ...)** gives access to the models of many providers behind a single contract with your cloud provider, which also facilitates access management, logging and compliance. New models are not always immediately available though and it strengthens the lock in with your cloud provider.
- **API access via serverless third party severless inference providers (OVH Cloud, Scaleway, Ionos, ...)** serve open-source models and charge per token. While not giving access to the latest and greatest, many of these companies operate in Europe and are fully subject by the data retention laws there.
- **Mistral** is the sole model provider that resides in Europe and is often the provider of choice there.


Put together, the large number of API providers strongly decrease supply chain risk. Pay-per-use is ideal for small and spiky volumes and enable production use cases, unlike the subscriptions.

### 3. Reserved capacity


### 4. Ownership


### 5. Other use cases
Bundled in applications, pay with your data




## Portfolio of AI model delivery mechanisms


## Conclusion:



