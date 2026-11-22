---
title: "A 'quite terrible' use of your computer: how to get started with local AI"
date: 2026-11-22 09:00:00 +0200
categories: [build-log]
series: data-ai-engineering
chapter: stack
part: 1
quote: "From a global allocation perspective, actually, it's quite terrible to have everybody have a bunch of the world's limited supply of DRAM sitting in their own computer and being used 5% of the time."
quote_author: "Sam Altman, Cheeky Pint podcast (2026)"
quote_source: "https://www.tomshardware.com/tech-industry/data-centers/openai-ceo-says-its-quite-terrible-for-your-pc-to-have-ram-sam-altman-says-data-centers-offer-better-utilization"
concepts:
  - local-ai
excerpt: "Claude can do a lot of my work. What does that dependence mean for my career or business, and which other ways of getting AI reduce the risk?"
tags:
  - local-ai
  - ai-engineering
---

## Introduction
In my previous blog post, I have discussed why capability is not the only factor in deciding which AI to use. Some tasks, for example, involve data that you don't want to share with an AI provider. Examples are medical information, financial information or sensitive schematics for that million dollar machine that you are building. 

In those cases, running an AI locally is the only viable way to benefit. However, most sources on the internet are developers that:
1. have really strong computers (e.g. 128 GB RAM), which are incredibly expensive right now.
2. don't mind using powerful, but not very user-friendly programs to run these models.

In this blog, I wanted to find out what I can do with my existing equipment:
- A mid-range self-assembled gaming PC
- a consumer laptop bought a few years ago for 700 euros

As an example, I'll use my local AI's to do a simple, but useful task: meeting notes summarization.
The blog covers the following:
- The important computer specifications for local AI and where to check them.
- Which small AI models can fit on your computer and how good they are.
- Setting up and using your first model in LM studio, a beginner friendly program for local hosting of AI models.
- Testing how the AI model on the gaming PC performs on the meeting notes task
- Testing how the AI model on the laptop performs on the meeting notes task





## (V)RAM is what matters for running local AI.


## Local weight models 



## Setting up your first model in LM studio


## The meeting notes task on a gaming PC


## The meeting notes task on a consumer laptop


## Conclusion
- The computer you own can already run local AI for simple but useful tasks.
- Better hardware does buy you better performance.
- If the current assistant covers your needs, there is no need to upgrade your PC. This blog will then be all you need.
- If you do need more performance, the next reasonable question would be: Is this the best model I can run on this hardware? In the next post, I'll do a deep dive on how to run the optimal model for your machine and dive deep into quantization, Mixture-of-Expert models and expert offloading and more, which collectively allow you to run more powerful machines than you would otherwise be able to do.
- If you like this post, leave a comment. I would like to know what machine you have and which model ended up working the best for you.