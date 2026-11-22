---
title: "Resistance is futile: the dangers of depending on frontier intelligence"
date: 2026-11-14 09:00:00 +0200
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

In a [previous post](TODO), I treated Claude Code Opus 5.5 like a data engineering expert and let it perform real-world data engineering tasks with nothing more than an interview at the start and saw impressive results.

The obvious next question is: **Should Claude do all the (data) engineering tasks it is capable off?**
While the "can" question can be answered using evals and measurements, the "should" question is more ambiguous and touches on costs (human, social, ...) that are not part of some standardized tests.
For that reason, I'll focus on the more manageable subquestion: **What strategic reasons could prevent a business from going all-in on Claude for data engineering?**


## The approach
The AI employee metaphor is helpful when deciding how to use these new assistants, but they gloss over very distinct relationships a business can have with them:
1. AI as a slave: The assistant is an owned asset and the business can use them as it sees fit. Not very moral, if you consider AI as an intelligent entity. I'll sidestep that moral debate for now and point out that this relation has the best business continuity guarantee.
2. AI as an employee: The assistant is not owned by the business, but the business owns critical knowledge of its business processes and new employees can come and go.
3. AI as a contractor: The business buys AI services from an agency, which may retain the knowledge internally, decide what work the contractor may take on or call them back, especially if there are few other agencies providing their services.

Using Claude is firmly category 3, which means businesses are essentially buying work from Anthropic and work has become a critical input for companies, similar to electricity, oil and others. So I propose we should treat it like procurement treats any other critical input.
Hence, this post is structured in the following way:

1. **Is the risk real?**: I'll look at incidents where AI suppliers changed the terms and why economics suggests this was no accident.
2. **A mental framework for procuring AI** I'll explore an extended version of the Kraljic matrix, which sorts purchases by how much they matter to you as a business (impact) and how risky their supply is (supply risk) and discuss the strategies that were derived for each combination.
3. **Measuring supply risk**: I'll investigate the different ways of buying AI, how many suppliers each has and where each method shines in the Kraljic matrix.
4. **Measuring impact** I'll reason through which the impact of my own tasks and what would happen if Claude disappeared tomorrow.
5. **Conclusion and next step**

### Frontier labs are not reliable suppliers in general
AI procurement risks are anything but imaginary:

| What the supplier did | What happened to me | In the news |
|---|---|---|
| **Cut access** | Fable 5 was switched off for about three weeks because of US export controls.[^fable] Mythos 5 came back only for approved US organisations.[^fable-restored] | The Pentagon labelled Anthropic a supply chain risk.[^pentagon] Providers cut off customers over their terms[^openai] [^bytedance] [^openai-regions] or for business reasons.[^windsurf] [^meta] |
| **Decided what is legitimate work** | My question about genome foundation models was moved from Opus 5.5 to the older Opus 5.[^fallback] | ChatGPT moves emotionally sensitive conversations to a stricter model.[^safety-routing] |
| **Wrote opinionated rules** | Anthropic's terms forbid me to use Claude to build a competing product.[^terms] | Anthropic itself launched Claude Code, which competes with tools built on Claude, such as Windsurf.[^windsurf] |
| **Changed the terms** | - | Anthropic's consumer terms now allow chats to be used for training; existing users had to accept or stop using Claude.[^consumer-terms] |
| **Changed how terms are enforced** | - | Using a subscription in a third-party tool worked for months, until providers started enforcing their terms.[^oauth] [^antigravity] |
| **Changed prices and limits** | For now, I can work around the limits I have, but I have seen per token pricing rather than per request pricing come into picture. | Cursor replaced 500 fast requests a month with a $20 usage budget.[^cursor] Anthropic added weekly limits to Claude Code.[^weekly-limits] |
| **Changed the quality** | - | Bugs and changed defaults made Claude worse for weeks.[^postmortems] |
| **Could not keep data out of reach** | - | Data stored in the EU is accessible for US authorities.[^cloud-act] |

None of this cost me much for now. While we can debate where providers should draw their lines, it is clear that *they* are drawing them. And prices may only be at the start. Uber rides were cheap in the early years, when it was a startup company and investors paid the difference. Once that subsidy ended, prices rose sharply.[^uber-fares]

This is not bad luck. The more you build around one supplier, the more room that supplier has to raise prices or change the rules (economists call this hold-up) and a market with few suppliers gives each of them more bargaining power.[^tce] Regulators have drawn the same conclusion. DORA already requires EU banks and insurers to assess concentration risk and to keep tested exit plans for critical technology providers.[^dora] So AI is already considered a critical input, like electricity or oil and it needs a procurement strategy.



### Analyzing supply chain risk

The sourcing options give me one axis of Kraljic's matrix: **supply risk**. That risk depends on how much intelligence a task needs. Work that needs the **ceiling** of intelligence, the best closed models, has only a few suppliers. Work that runs on the **floor**, models that many providers serve, has many.

The second axis is **profit impact**: how much it hurts if this work stops or gets more expensive. For AI, that comes down to whether a task is **core** or **edge**:

- **Core** is the work you earn your money with, as a business or as a professional.
- **Edge** is everything else: supporting work, and new experiments that haven't proven themselves yet.

One axis comes from the market, the other from your own work. Together they give four quadrants, and Kraljic's real contribution is that each quadrant comes with its own strategy.[^kraljic]

![The Kraljic matrix: profit impact against supply risk, with leverage, strategic, non-critical and bottleneck items]({{ '/assets/images/posts/kraljic-matrix.png' | relative_url }})

| Quadrant | For AI, this is | Kraljic's advice | What I do with it |
|---|---|---|---|
| **Strategic** (high impact, high risk) | Core work that needs the ceiling | Plan for contingencies, decide what to make yourself | Keep using the frontier, but build a working fallback on the floor |
| **Leverage** (high impact, low risk) | Core work that runs on the floor | Use your buying power, switch suppliers | Shop around: send the work to whichever provider is cheapest and best today |
| **Bottleneck** (low impact, high risk) | Edge work that needs the ceiling | Secure supply, keep a backup | Depend on the frontier, but keep it out of production. A plan I can cancel is my backup |
| **Non-critical** (low impact, low risk) | Edge work that runs on the floor | Standardise, keep the effort low | Whatever is cheapest and easiest |

The disqualifiers come before the matrix. They strike options off the list for a task before anything is scored.

This brings me to a simple way to score AI solutions from a procurement lens:

| Layer | Question | Criteria | Example | Borrowed from |
|---|---|---|---|---|
| **1. Disqualifiers** (pass or fail) | Can this option be used at all? | Data protection and data residency (GDPR), sector rules, export controls, the provider's usage policy | I can't send sensitive personal data to an external provider like Anthropic. For those tasks I need another supplier, however good or cheap Claude is. | Supplier qualification[^qualification] |
| **2. Supply chain risk** (low to high) | How much power does the supplier have over me? | Price and rate limits, access, outages, models being retired, silent quality changes, lock-in | Windsurf lost direct access to Claude with less than five days notice.[^windsurf] | Kraljic: supply risk[^kraljic] |
| **3. Value** (low to high) | What do I get for my money? | Capability, effort, cost | AT&T found open models 56% cheaper on some tasks, for about 2% lower quality.[^pragmatic] | Kraljic: profit impact[^kraljic] |

With this framework, I can evaluate case-by-case when a certain AI solution fits for a certain task based on disqualifiers, my dependence and getting a bang for my buck.
The next question then becomes: **What are my alternatives?**

## Where frontier models fit and where they don't


## What next

Moving the core to the floor has a price: engineering time, smaller steps, evaluations, a harness that can switch models. For a task I run once, that will never pay off. For a task I run every day, it probably will.

That is the next post: **what does an AI task really cost, and when does building the fallback pay for itself?**

[^fable]: (checked) [Implicator, 13 June 2026](https://www.implicator.ai/us-export-controls-force-anthropic-to-pull-fable-5-and-mythos-5-days-after-launch/): order of 12 June, Anthropic disabled both models for all customers. Lifted 30 June, restored 1 July: [Al Jazeera](https://www.aljazeera.com/economy/2026/7/1/us-lifts-restrictions-on-powerful-ai-models-fable-mythos-anthropic-says) · [CoinDesk](https://www.coindesk.com/tech/2026/07/01/anthropic-restores-ai-models-fable-mythos-after-the-u-s-lifts-export-controls)
[^fable-restored]: (checked, secondary) Mythos 5 reintroduced only to approved US organisations: [GovConWire](https://www.govconwire.com/articles/anthropic-fable-5-mythos-5-access-restored) · [CoinDesk](https://www.coindesk.com/tech/2026/07/01/anthropic-restores-ai-models-fable-mythos-after-the-u-s-lifts-export-controls). TODO: confirm in Anthropic's own announcement.
[^glasswing]: (checked) Project Glasswing, April 2026: Mythos Preview limited to 12 launch partners (including AWS, Apple, Google, Microsoft, NVIDIA, JPMorgan Chase) and about 40 further organisations. [TechCrunch](https://techcrunch.com/2026/04/07/anthropic-mythos-ai-model-preview-security/) · [InfoQ](https://infoq.com/news/2026/04/anthropic-claude-mythos)
[^pentagon]: (notes) [Axios](https://www.axios.com/2026/02/27/anthropic-pentagon-supply-chain-risk-claude) · [Mayer Brown](https://www.mayerbrown.com/en/insights/publications/2026/03/pentagon-designates-anthropic-a-supply-chain-risk-what-government-contractors-need-to-know) · [CNBC](https://www.cnbc.com/2026/09/25/pentagon-anthropic-ai-risk-appeals-court.html)
[^windsurf]: [The Decoder](https://the-decoder.com/anthropic-cuts-claude-access-for-windsurf-after-openais-3b-takeover-news/) · [TechTalks](https://bdtechtalks.com/2025/06/11/anthropic-cuts-windsurf-access-to-claude/)
[^openai]: (checked) August 2025, first reported by WIRED. [TechCrunch](https://www.techcrunch.com/2025/08/02/anthropic-cuts-off-openais-access-to-its-claude-models)
[^bytedance]: (checked, secondary; original reporting by The Verge, 15 December 2023) OpenAI suspended ByteDance's account while investigating claims that it used the API to build a rival model; ByteDance said its use was licensed and the GPT data had been removed. [The Decoder](https://the-decoder.com/openai-bans-tiktok-company-bytedance-from-chatgpt-due-to-possible-data-theft/)
[^openai-regions]: (checked) June 2024 email: "We will be taking additional measures to block API traffic from regions that are not on our supported countries and territories list starting on July 9." Unsupported regions include mainland China, Hong Kong, Russia and Iran; OpenAI gave no further reason. [The Register](https://www.theregister.com/2024/06/25/openai_unsupported_countries/)
[^meta]: (checked, secondary; original reporting by the Financial Times, 28 June 2026) From around March 2026, Google could not supply the Gemini capacity Meta wanted; Meta moved workloads to its own model. Both declined to comment. [The Next Web](https://thenextweb.com/news/google-caps-meta-gemini-compute-shortage)
[^antigravity]: (checked) February 2026: Google cut off Antigravity users who accessed Gemini through the third-party tool OpenClaw, citing "malicious usage" and its terms of service. [VentureBeat](https://venturebeat.com/ai/google-clamps-down-on-antigravity-malicious-usage-cutting-off-openclaw-users)
[^consumer-terms]: (checked) Anthropic, ["Updates to Consumer Terms and Privacy Policy"](https://www.anthropic.com/news/updates-to-our-consumer-terms), 28 August 2025: applies to Free, Pro and Max; five-year retention "if you allow us to use your data for model training", otherwise 30 days; existing users "have until October 8, 2025 to accept the updated Consumer Terms and make your decision". Does not apply to Commercial Terms or API use.
[^oauth]: (notes) [VentureBeat](https://venturebeat.com/technology/anthropic-cracks-down-on-unauthorized-claude-usage-by-third-party-harnesses) · [AlternativeTo](https://alternativeto.net/news/2026/2/anthropic-officially-bans-using-subscription-authentication-for-third-party-claude-use)
[^postmortems]: (notes) [Aug/Sep 2025](https://www.anthropic.com/engineering/a-postmortem-of-three-recent-issues) · [Mar-Apr 2026](https://www.anthropic.com/engineering/april-23-postmortem). Bugs and configuration changes, not secret model swaps.
[^fallback]: (checked) Opus 5.5 falls back to Opus 5 on dual-use requests in areas such as virology, toxicology and molecular design. On by default, shown with a notice, can be turned off (the conversation then pauses). [Claude Help Center](https://support.claude.com/en/articles/16049681-why-claude-switched-models-in-your-conversation-with-opus-5-or-opus-5-5)
[^safety-routing]: (checked) September 2025: ChatGPT detects emotionally sensitive conversations and switches them, per message, to GPT-5-thinking. Some users called it overly cautious; OpenAI's Nick Turley: "Routing happens on a per-message basis; switching from the default model happens on a temporary basis." [TechCrunch, 29 September 2025](https://techcrunch.com/?p=3051550) · [TechRadar](https://www.techradar.com/ai-platforms-assistants/chatgpt/openai-responds-to-furious-chatgpt-subscribers-who-accuse-it-of-secretly-switching-to-inferior-models)
[^tac]: (checked) Researchers say guardrails "prevent them from doing legitimate work". On 19 August 2026 some lost access to OpenAI's Trusted Access for Cyber programme; OpenAI blamed "a technical issue affecting a limited number of users" and asked them to re-verify. [TechCrunch](https://techcrunch.com/2026/08/19/researchers-complain-that-openai-revoked-their-access-to-limited-cyber-program/) · programme: [OpenAI](https://openai.com/index/scaling-trusted-access-for-cyber-defense/)
[^uber-fares]: (checked, secondary; original: Henry Grabar in Slate, May 2022) Uber lost more than $30 billion in five years, "an enormous, investor-fueled subsidy of ride-hailing habits"; average Uber prices rose 92% between 2018 and 2021 (Rakuten data), 45% between 2019 and 2022 (Bloomberg). CEO Dara Khosrowshahi to staff: "Those days are over." [Excerpt on The Low-Down](https://www.thelowdownblog.com/2022/05/why-decade-of-cheap-uber-rides-is-over.html). TODO: link the Slate original.
[^cursor]: (checked) June 2025: Cursor Pro went from 500 fast requests plus unlimited slow ones to $20 of usage at API rates; users hit limits after a few prompts or were billed unexpectedly. CEO Michael Truell: "We recognize that we didn't handle this pricing rollout well and we're sorry." Refunds offered. [TechCrunch](https://techcrunch.com/2025/07/07/cursor-apologizes-for-unclear-pricing-changes-that-upset-users/) · [Cursor](https://cursor.com/blog/june-2025-pricing)
[^weekly-limits]: (checked) Announced 28 July 2025, from August 2025: weekly caps on Claude Code for Pro and Max, on top of the five-hour limits, expected to affect "less than 5% of subscribers"; Anthropic cited users running it "continuously in the background, 24/7" and account sharing. [Engadget](https://www.engadget.com/ai/anthropic-is-rate-limiting-claude-code-blaming-some-users-for-never-turning-it-off-211134730.html)
[^cloud-act]: (notes) [The Register](https://www.theregister.com/2025/07/25/microsoft_admits_it_cannot_guarantee/) · [Forbes](https://www.forbes.com/sites/emmawoollacott/2025/07/22/microsoft-cant-keep-eu-data-safe-from-us-authorities/)
[^kraljic]: (checked) Peter Kraljic, ["Purchasing Must Become Supply Management"](https://hbr.org/1983/09/purchasing-must-become-supply-management), Harvard Business Review, September 1983. Kraljic scores purchases on supply risk and profit impact; I use those as my second and third layers. The pass/fail first layer and the combination are mine. Strategies per quadrant (memory, check against the paper): strategic: demand forecasting, long-term supply relationships, make-or-buy decisions, contingency planning; leverage: full use of purchasing power, vendor selection, substitution; bottleneck: volume insurance, backup plans; non-critical: standardisation, efficient processing. The translation to AI is mine.
[^qualification]: (checked, secondary) [ISO 9001:2015](https://www.iso.org/standard/62085.html), clause 8.4.1: the organisation must determine and apply criteria for the evaluation, selection, monitoring of performance and re-evaluation of external providers. The standard itself is paywalled; clause summary: [Ecesis](https://www.ecesis.net/ISO-9001/Control-of-External-Providers.aspx) · [CertBetter](https://certbetter.com/blog/what-is-supplier-evaluation-in-iso-9001-and-how-do-you-do-it).
[^tce]: (checked, secondary) Oliver E. Williamson, *The Economic Institutions of Capitalism: Firms, Markets, Relational Contracting*, Free Press, 1985: investments that are worth much inside a relationship but little outside it (asset specificity) expose the investor to renegotiation. Overview: [Wikipedia, "Asset specificity"](https://en.wikipedia.org/wiki/Specific_asset). The term hold-up is usually traced to Klein, Crawford and Alchian (1978). Michael E. Porter, ["How Competitive Forces Shape Strategy"](https://hbr.org/1979/03/how-competitive-forces-shape-strategy), Harvard Business Review, March–April 1979: the bargaining power of suppliers is one of the forces that shape competition.
[^dora]: (checked, secondary) Regulation (EU) 2022/2554, [Article 28](https://www.springlex.eu/en/packages/dora/dora-regulation/article-28/) (exit strategies) and [Article 29](https://advisera.com/dora-regulation/preliminary-assessment-of-ict-concentration-risk-at-entity-level/) (concentration risk). TODO: link EUR-Lex.
[^ai-act]: (checked, secondary) Commission guidelines for general-purpose AI models, 18 July 2025: a downstream modifier generally becomes the provider when the modification uses more than a third of the original training compute; parameter-efficient fine-tuning is unlikely to reach this. [CMS](https://cms.law/en/swe/legal-updates/general-purpose-ai-models-obligations-and-roles-for-providers-and-downstream-modifiers-under-the-eu-ai-act) · [WilmerHale](https://www.wilmerhale.com/en/insights/blogs/wilmerhale-privacy-and-cybersecurity-law/20250724-european-commission-issues-guidelines-for-providers-of-general-purpose-ai-models)
[^pragmatic]: (checked) [The Pragmatic Engineer, "Tech companies move to open AI models"](https://blog.pragmaticengineer.com/the-pulse-tech-companies-move-to-open-ai-models/): Uber's cost per AI session down 52%; AT&T 56% cheaper on some tasks for about 2% lower quality; Pinterest CEO: "less than 8% of the cost of comparable closed proprietary models".
[^terms]: (checked) [Anthropic Commercial Terms](https://www.anthropic.com/legal/commercial-terms), section D.4.
[^epoch]: (notes) [Epoch AI: open vs closed gap](https://epoch.ai/data-insights/open-closed-eci-gap)
