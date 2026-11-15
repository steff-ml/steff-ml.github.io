---
title: "The landlord decides: the dangers of depending on Claude"
date: YYYY-MM-DD 09:00:00 +0200
categories: [build-log]
series: data-ai-engineering
chapter: stack
part: 1
quote: "Your margin is my opportunity."
quote_author: "Jeff Bezos (attributed)"
quote_source: "https://quoteinvestigator.com/2019/01/13/margin/"
concepts:
  - ai-gateway
excerpt: "Claude can do a lot of my work. What does that dependence mean for my career or business, and which other ways of getting AI reduce the risk?"
tags:
  - ai-engineering
  - ai-sourcing
---
{% comment %}
AI DRAFT (written with the exponential-post skill, based on outline.md as of
2026-11-13). Not reviewed yet. Check every claim, then move this file to
_drafts/ or _posts/ and delete this comment.

Fact checks that changed what you told me (please confirm the new wording):
- FABLE: the export order (12 June 2026) hit ALL customers, not only non-US ones:
  Anthropic switched Fable 5 off for everyone until 1 July. So "competitors
  elsewhere kept it" is not supported. What IS supported: when access returned,
  Mythos 5 came back only for approved US organisations, and Mythos Preview was
  given first to a small group of partners (Project Glasswing). The draft uses that.
- BIOLOGY FALLBACK: Opus 5.5 switches to Opus 5 on dual-use biology requests by
  default, with a notice, and the switch can be turned off in Settings. So
  "without consent" is too strong; the draft says "by default". TODO(steff):
  did you see the notice / know about the setting?
- SIERRA: the public figure is a self-reported 5x "on some tasks", using Claude
  Code AND Codex. The 3-20x figure was not found.
- PINTEREST: "<8%" is the CEO's figure as quoted by The Pragmatic Engineer.
- INTERCOM: its own research shows a fine-tuned Qwen 14B matching GPT-4.1 on an
  extraction task; the "$250k/month" figure only appears in a podcast blurb.
- AT&T: "56% cheaper for ~2% lower quality" comes from The Pragmatic Engineer;
  "up to 90%" from AT&T's own blog. The WSJ original was not found.
- Microsoft Copilot dropped (not checked, not needed).
- Trimmed 2026-11-13: dangers told once (full story in the incidents), case-study
  cells shortened (details in footnotes), Vercel/Gartner aggregate data moved to
  the cost-per-task outline.

Open items:
- TODO(steff): revise portfolio rows (in use vs planned).
- TODO(steff): title. Post 2's draft title also uses "Own the core, rent the edge";
  that phrase now lands in this post, so post 2 needs a new title.
- TODO: concept page "AI sourcing options" and the link to the clinical-trials post.
- Reference legend: (checked) = opened or confirmed by search on 2026-11-13;
  (notes) = your research notes (checked 2026-09-25); (memory) = from memory,
  check before publishing.
{% endcomment %}

## The question

In the [previous post](TODO) I let Claude Code ingest, clean and visualise clinical trial data with nothing more than an interview at the start. It worked remarkably well.

It also made me look at the other side of the coin. The more of my work Claude does, the more my work depends on one provider. There are many ways to think about that: what it does to how we learn, to our jobs, to society. This post takes one lens only: **what does it mean for my career or my business?**

So the question is: **what are the dangers of letting Claude do my work, and are there alternatives?**

## The approach

This is desk research, not a test. Comparing models on real tasks needs evaluations and a gateway, and that is for later posts.

**First, the dangers.** I only counted a danger if I could point to something that actually happened:

- **Access gets cut**, by a government[^fable] [^pentagon] or by the provider itself.[^windsurf] [^openai]
- **Terms change**, such as subscriptions no longer working in third-party tools.[^oauth]
- **Quality changes underneath you**, through bugs, changed defaults or deliberate routing to another model.[^postmortems] [^fallback]
- **Prices and limits change** once you depend on a tool.[^uber]
- **Data stored in the EU is not out of reach** of US authorities.[^cloud-act]

I ran into several of these myself. More on that below.

**Then, a way to score the options.** I sorted the criteria into three layers:

1. **Disqualifiers** (pass or fail): can this option be used at all? Data protection and data residency (GDPR), sector rules, export controls and the provider's usage policy can each rule an option out.
2. **Supply risk**: how much power does the supplier have over me? Price and rate limits, access, outages, models being retired, silent quality changes, lock-in.
3. **Value**: what do I get? Capability, effort and cost.

None of this is new. Procurement has weighed supply risk against impact since Peter Kraljic's matrix in 1983.[^kraljic] Economists have long explained why depending on one supplier gets expensive: the more you invest in a relationship that only works with that supplier, the more room the supplier has to raise prices (what Oliver Williamson called hold-up), and a supplier with few competitors has more bargaining power.[^tce] Regulators know it too. The EU's DORA regulation requires banks and insurers to assess concentration risk and to have tested exit plans for critical technology providers.[^dora]

## What happened

### Five ways to get AI

From least to most hands-on:

1. **Subscription**: a monthly plan such as [Claude](https://claude.com/pricing), ChatGPT or Gemini. Cheap and easy, but you get the provider's apps on the provider's terms.
2. **API, closed model**: pay per token, directly from a lab ([Anthropic](https://claude.com/pricing#api), [OpenAI](https://openai.com/api/pricing/), [Google](https://ai.google.dev/gemini-api/docs/pricing)) or through a hyperscaler ([AWS Bedrock](https://aws.amazon.com/bedrock/pricing/), [Microsoft](https://azure.microsoft.com/en-us/products/ai-foundry/models)).
3. **API, open model**: pay per token for open-weight models, for example at European providers such as [OVHcloud](https://www.ovhcloud.com/en/public-cloud/ai-endpoints/), [Scaleway](https://www.scaleway.com/en/generative-apis/) and [Ionos](https://cloud.ionos.com/managed/ai-model-hub). [Mistral](https://mistral.ai/pricing#api-pricing) offers a model family that is partly open-weight.
4. **Rented compute**: renting GPUs and serving an open-weight model yourself.
5. **Own hardware**: running open-weight models on machines you own.

Details per option are on a separate page: [AI sourcing options](TODO).

### The scored table

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

✓ = strong, ~ = depends on provider, volume or use case, ✗ = weak. My own judgement, and a simplification.

The surprise was where the line falls. I expected the big split to be between renting and owning. It isn't: it's between **closed and open models**. Paying per token for Claude and paying per token for an open model at a European provider look the same on the invoice, but carry very different risks. An open model can be moved to another provider, or to your own machine, when the terms change. A closed model can't.

The EU AI Act is not in the table, because as a user of AI your obligations barely depend on how you get it. One exception: if you modify a general-purpose model substantially, you can become its provider in the eyes of the law. The Commission's rule of thumb is more than a third of the original training compute, which ordinary fine-tuning doesn't come close to.[^ai-act]

### What companies do

If the trade-off is real, companies should land in different places. They do. Most figures below are reported by the companies themselves.

| Strategy | Examples |
|---|---|
| **All-in on frontier models** | Cognizant has put more than 25,000 people on Claude.[^cognizant] Deloitte is rolling Claude out to its whole workforce.[^deloitte] Sierra reports 5x more output on some tasks with Claude Code and Codex.[^sierra] |
| **Both at once** | Uber spent its 2026 AI tools budget in four months,[^uber] then moved some tasks to open models and halved its cost per AI session.[^pragmatic] |
| **Open by default, frontier as the exception** | Revolut's default coding model is a self-hosted open model, at a tenth of its former bill.[^revolut] Intercom matched GPT-4.1 on one task with a fine-tuned 14B Qwen model.[^intercom] AT&T routes each task to the smallest model that can do it, with savings of up to 90%.[^att] |
| **Own models** | Pinterest pays less than 8% of the closed-model cost per transaction.[^pragmatic] C3 AI's fine-tuned model beats frontier models on its own software, faster.[^c3] |

Three things stand out:

- **The work that moves is high-volume or specific to the business.** Revolut's everyday coding, Intercom's support pipeline, AT&T's telecom work, C3 AI's own software. The hardest and newest work stays with frontier models.
- **Almost nobody moves completely.** Companies decide per task.
- **The all-in examples are often services firms.** For Cognizant and Deloitte, betting on one provider is partly what they sell.

### My portfolio, and the incidents behind it

My own setup came out of incidents, not theory. None of them cost me much.

- **A question was switched to a weaker model.** When I asked about genome foundation models, Claude moved my conversation from Opus 5.5 to Opus 5. Anthropic does this by default for dual-use biology.[^fallback] I understand why the safeguard exists. But it showed me that the provider decides which model answers my question.
- **I lost access to frontier models.** Fable 5 was unavailable for about three weeks because of the export controls.[^fable] When access came back, Fable returned for everyone, but Mythos 5 only for approved US organisations.[^fable-restored] Its preview had gone first to a small group of large partners.[^glasswing]
- **The rules point one way.** Anthropic's terms forbid customers from using its services to build a competing product or to reverse engineer them.[^terms] The provider itself has no such limit: it launched Claude Code, which competes with coding tools like Windsurf that were built on its models.[^windsurf]
- **Prices haven't changed for me yet.** But Uber shows how it works: once your work depends on a tool, the bill grows with your use.

I am not judging whether these rules are right. That is another post. For this one, the effect is what matters. Each restriction moved capability away from me and towards someone else: the provider, its partners, or companies the government favours.

{% comment %} TODO(steff): revise rows, mark what is in use and what is planned. {% endcomment %}

| Way of getting AI | Provider / model | Used for | Reduces which danger |
|---|---|---|---|
| **Subscription** | Claude | Brainstorming and infrastructure coding | none: easy to replace with another lab's plan, because nothing in production depends on it |
| **Subscription** | Gemini | Video and image analysis | none: some loss of capability if it disappears |
| **API, open model** | Qwen via OVHcloud and Scaleway | All-round production work | Lock-in and access: the same model at several EU providers |
| **API, open model** | DeepSeek via Scaleway | Cheap long-context coding and review | Price, but fewer providers, so nothing critical |
| **API, closed / partly open** | Mistral | Work where a European provider matters | Data residency |
| **Own hardware** | Small open-weight models | Learning and routine tasks | Everything: this *is* the fallback |

### What I couldn't settle

The honest summary: **the losses were minimal, but it did spook me.** The case for diversifying rests on what *could* happen, not on what has cost me money so far. That's how insurance works, but it is a weaker argument than a disaster.

And on paper I can't tell whether a cheaper model can do my work. Whether an open model at a European provider handles the clinical trials pipeline as well as Claude depends on success rates I haven't measured yet.

## The lesson

**Own the core, rent the edge.**

- **Core** is what sets you apart. You should own the knowledge behind it.
- **Edge** is everything that doesn't set you apart, plus new experiments that haven't proven themselves yet.

Renting is not wrong. It is flexible, cheap and the right choice for exploring. But rent builds no asset, and the landlord sets the terms. Everything I ran into came down to that. Relying fully on Claude means renting what should be the core of my business, from a provider that can change the price, the model or the access, and that may one day build what I build.

So think of your knowledge, personal and business, as **capital**, and invest in it deliberately. Use AI to automate what is not core to your offering. That applies at two levels:

- **For a business:** own your processes, your data and the knowledge of how your work gets done.
- **For a career:** own the understanding that clients pay you for.

The timing is good, because **owning the core has never been this cheap.** According to Epoch AI, the best open-weight models trail the closed frontier by only about four months.[^epoch] Owning no longer means training your own model. You can own the model and still rent the compute.

One more thing the case studies show: **tasks move.** An experiment starts at the edge, on a frontier model. Once it proves itself and the volume grows, it becomes core, and it starts to pay to own it.

## What next

"Core is what sets you apart" is easy to say. It is much harder to apply to a single task. Is a daily data-cleaning job core because I run it constantly, or edge because anyone could do it?

That is the next post: **what is core, what is edge, and how much of that line can numbers decide?**

[^fable]: (checked) [Implicator, 13 June 2026](https://www.implicator.ai/us-export-controls-force-anthropic-to-pull-fable-5-and-mythos-5-days-after-launch/): order of 12 June, Anthropic disabled both models for all customers. Lifted 30 June, restored 1 July: [Al Jazeera](https://www.aljazeera.com/economy/2026/7/1/us-lifts-restrictions-on-powerful-ai-models-fable-mythos-anthropic-says) · [CoinDesk](https://www.coindesk.com/tech/2026/07/01/anthropic-restores-ai-models-fable-mythos-after-the-u-s-lifts-export-controls)
[^fable-restored]: (checked, secondary) Mythos 5 reintroduced only to approved US organisations: [GovConWire](https://www.govconwire.com/articles/anthropic-fable-5-mythos-5-access-restored) · [CoinDesk](https://www.coindesk.com/tech/2026/07/01/anthropic-restores-ai-models-fable-mythos-after-the-u-s-lifts-export-controls). TODO: confirm in Anthropic's own announcement.
[^glasswing]: (checked) Project Glasswing, April 2026: Mythos Preview limited to 12 launch partners (including AWS, Apple, Google, Microsoft, NVIDIA, JPMorgan Chase) and about 40 further organisations. [TechCrunch](https://techcrunch.com/2026/04/07/anthropic-mythos-ai-model-preview-security/) · [InfoQ](https://infoq.com/news/2026/04/anthropic-claude-mythos)
[^pentagon]: (notes) [Axios](https://www.axios.com/2026/02/27/anthropic-pentagon-supply-chain-risk-claude) · [Mayer Brown](https://www.mayerbrown.com/en/insights/publications/2026/03/pentagon-designates-anthropic-a-supply-chain-risk-what-government-contractors-need-to-know) · [CNBC](https://www.cnbc.com/2026/09/25/pentagon-anthropic-ai-risk-appeals-court.html)
[^windsurf]: (checked) June 2025, under five days' notice; Jared Kaplan said it would be "odd" to sell Claude to OpenAI. Coverage links it to Anthropic's own Claude Code. [The Decoder](https://the-decoder.com/anthropic-cuts-claude-access-for-windsurf-after-openais-3b-takeover-news/) · [TechTalks](https://bdtechtalks.com/2025/06/11/anthropic-cuts-windsurf-access-to-claude/)
[^openai]: (checked) August 2025, first reported by WIRED. [TechCrunch](https://www.techcrunch.com/2025/08/02/anthropic-cuts-off-openais-access-to-its-claude-models)
[^oauth]: (notes) [VentureBeat](https://venturebeat.com/technology/anthropic-cracks-down-on-unauthorized-claude-usage-by-third-party-harnesses) · [AlternativeTo](https://alternativeto.net/news/2026/2/anthropic-officially-bans-using-subscription-authentication-for-third-party-claude-use)
[^postmortems]: (notes) [Aug/Sep 2025](https://www.anthropic.com/engineering/a-postmortem-of-three-recent-issues) · [Mar-Apr 2026](https://www.anthropic.com/engineering/april-23-postmortem). Bugs and configuration changes, not secret model swaps.
[^fallback]: (checked) Opus 5.5 falls back to Opus 5 on dual-use requests in areas such as virology, toxicology and molecular design. On by default, shown with a notice, can be turned off (the conversation then pauses). [Claude Help Center](https://support.claude.com/en/articles/16049681-why-claude-switched-models-in-your-conversation-with-opus-5-or-opus-5-5)
[^uber]: (checked, secondary; original reporting by The Information) [Fortune, 26 May 2026](https://fortune.com/2026/05/26/uber-coo-ai-spending-tokens-claude-code/) · [Outlook Business](https://www.outlookbusiness.com/corporate/uber-caps-ai-tool-usage-after-exhausting-yearly-budget-in-just-4-months) ($1,500 monthly cap per employee)
[^cloud-act]: (notes) [The Register](https://www.theregister.com/2025/07/25/microsoft_admits_it_cannot_guarantee/) · [Forbes](https://www.forbes.com/sites/emmawoollacott/2025/07/22/microsoft-cant-keep-eu-data-safe-from-us-authorities/)
[^kraljic]: (checked) Peter Kraljic, ["Purchasing Must Become Supply Management"](https://hbr.org/1983/09/purchasing-must-become-supply-management), Harvard Business Review, September 1983.
[^tce]: (memory) Oliver Williamson, *The Economic Institutions of Capitalism*, 1985 (asset specificity and hold-up); Michael Porter, ["How Competitive Forces Shape Strategy"](https://hbr.org/1979/03/how-competitive-forces-shape-strategy), Harvard Business Review, 1979 (supplier power).
[^dora]: (checked, secondary) Regulation (EU) 2022/2554, [Article 28](https://www.springlex.eu/en/packages/dora/dora-regulation/article-28/) (exit strategies) and [Article 29](https://advisera.com/dora-regulation/preliminary-assessment-of-ict-concentration-risk-at-entity-level/) (concentration risk). TODO: link EUR-Lex.
[^ai-act]: (checked, secondary) Commission guidelines for general-purpose AI models, 18 July 2025: a downstream modifier generally becomes the provider when the modification uses more than a third of the original training compute; parameter-efficient fine-tuning is unlikely to reach this. [CMS](https://cms.law/en/swe/legal-updates/general-purpose-ai-models-obligations-and-roles-for-providers-and-downstream-modifiers-under-the-eu-ai-act) · [WilmerHale](https://www.wilmerhale.com/en/insights/blogs/wilmerhale-privacy-and-cybersecurity-law/20250724-european-commission-issues-guidelines-for-providers-of-general-purpose-ai-models)
[^cognizant]: (checked) [Analytics India Magazine, 11 November 2026](https://analyticsindiamag.com/ai-news/cognizant-commits-25000-professionals-to-deploy-claude-in-enterprises)
[^deloitte]: (checked) October 2025. [Anthropic](https://www.anthropic.com/news/deloitte-anthropic-partnership) · [Deloitte](https://www.deloitte.com/us/en/about/press-room/deloitte-and-anthropic-collaborate-to-bring-safe-reliable-trusted-ai-to-commercial-government-organizations.html)
[^sierra]: (checked) Self-reported, January pilot. [Sierra, "AI-pilling our company"](https://sierra.ai/blog/ai-pilling-our-company-lessons-learned)
[^pragmatic]: (checked) [The Pragmatic Engineer, "Tech companies move to open AI models"](https://blog.pragmaticengineer.com/the-pulse-tech-companies-move-to-open-ai-models/): Uber's cost per AI session down 52%; AT&T 56% cheaper on some tasks for about 2% lower quality; Pinterest CEO: "less than 8% of the cost of comparable closed proprietary models".
[^revolut]: (checked) [AI Engineer talk](https://ai.engineer/talks/Ok6QdZHCcI8-revolut-cut-ai-coding-costs-90-self). The comparison doesn't match usage periods or include the cost of running the service.
[^intercom]: (checked) [Intercom (Fin) research, "David vs Goliath: are small LLMs any good?"](https://fin.ai/research/david-vs-goliath-are-small-llms-any-good/) · [Cost of serving LLMs](https://fin.ai/research/cost-of-serving-llms/)
[^att]: (checked) [AT&T, "The tokenomics equation", 23 July 2026](https://about.att.com/blogs/2026/the-tokenomics-equation.html)
[^c3]: (checked) [C3 AI blog](https://c3.ai/blog/why-c3-ai-fine-tunes-its-own-models-even-with-frontier-ai-this-strong)
[^terms]: (checked) [Anthropic Commercial Terms](https://www.anthropic.com/legal/commercial-terms), section D.4.
[^epoch]: (notes) [Epoch AI: open vs closed gap](https://epoch.ai/data-insights/open-closed-eci-gap)
