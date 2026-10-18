---
title: "Why I'm building this website"
date: 2026-09-25 09:40:00 +0200
categories: [thinking]
tags:
  - mission
  - open-data
---

# Introduction: The grand crises of AI
I believe we are witnessing one of the greatest technological revolutions of a generation. Yet we are completely bummed out about it.
This paradox I find fascinating and I believe comes from three major crises at the heart of this revolution:
- Workforce disruption: How can we navigate a workplace that is changing faster than we can handle?
- The concentration of power: How can we build AI as a technology that empowers regular people?
- The pursuit of meaning: How can we orient our careers towards meaningful endeavours in the AI age?

Each of these is a generational level challenge and comes on top of wars, famine and climate change.
I want to address them head-on here.

# Workforce disruption: How can we navigate a workplace that is changing faster than we can handle?
Project OT (short for Organizational Transformation) was a leaked secret project concocted by the leadership team of Meta (or Facebook if you want) to make their workforce AI-native. AI-native means in this context that they wanted to completely overhaul the processes inside their company inspired/frightened by the new workflows developed in new AI startups like Anthropic, OpenAI and the new AI startups. This included  significant reductions in team size, clubbing developer, UX designer, product manager etc in a new builder role, the project "Irreplaceable Talent" to identify "10x Engineers" and overall flatter organizational structures. 

![Project OT's proposed organizational structure](/assets/images/project_OT_organizational_structure.png)

To achieve this, Meta's management planned not only reductions in team size of up to 60%, but forced their employees to train the AI that would replace them. Software engineers were reassigned to provide input for the internal coding AI via programming puzzle exercises and all employees had mouse-tracking software installed on their computer. Meanwhile, the replacement tool wasn't working as well. Employees were burning tokens, but producing more questionable outputs. The number of incidents (service failures, potential data leaks, ...) rose by 40%. The resulting employee revolt and the disappointing results of the internally created AI caused the leadership team to back down after a 10% employee layoff, causing immense reputational damage.
While project OT was unique in its brutality, Meta was definitely not alone in thinking along these lines. This blog by Anthropic for example shows how they organize their AI-native teams and do note the similarities with project OT's structure. 

This story encapsulates to me the true nature of the AI revolution at work. It shows a pressure (and in some cases desire) to use AI to cut costs . It shows how job roles become less differentiated into "builder" roles that leverage AI. Above all, it shows that some companies are willing to push their goals, irrespective of the human cost and creating pressure on more upstanding companies to do the same. 

As for what to do, I only have partial answers. Employee organization and unionization can be a great and peaceful way to start conversations with employers on the new nature of work and were clearly effective in Meta's case. Your coworkers are your friends. As for the deeper change to job roles, I think education and experimentation can help. This is the first raison d'être of this website: What happens when we do take on an AI builder role? What works and what doesn't? Where am we still needed and where not? 
These questions can only be resolved by doing and feeling, not thinking. 


The impact of AI on work is in my opinion understated.

https://www.reuters.com/investigations/mark-zuckerberg-had-bold-plan-replace-meta-staff-with-ai-heres-how-it-imploded-2026-08-26/

# Power concentration: How can we build AI as a technology that empowers regular people?
Anthropic had a bad few weeks a couple of months ago. It started off ok though. They had just gained some reputational credibility by refusing to lend Claude models for domestic surveillance and autonomous weapons (although using it for other warfare purposes is fair game I guess). This alone tells an entire story of where the bar is right now, but I digress. They were bringing out their biggest model Mythos, a model so powerful it initially could not be trusted in the hands of regular mortals. A bit later, they distributed a variant of it for the masses called Fable. The launch turned out to be a dud for me, but interestingly not because of the model itself. The Trump government imposed an export ban on Fable a few days after its release, meaning I had only 2 or 3 real days I could test it. I love biotechnology so I wanted to point it to questions in that field, only to find that Anthropic had installed guardrails that prevented me from asking any biotechnological questions. The guardrails were a highly opinionated design choice: they were afraid that someone would make a bioweapon with their model, but clearly overshot. Since then, we have seen a deluge of news stories of bad business practices of leading AI companies:
- OpenAI and Anthropic were accused of manipulating the performance of their models to make them cheaper to run.
- OpenAI has been accused of stealing the method that produced a proof for the Navier-Stokes problem.
- Anthropic opens a molecular biology lab directly controlled by Claude, whilst restricting our use on it as mentioned above.
- Both companies have been engaging in some puzzling instances of doomer marketing:
  - Superintelligent AI's will kill us all and we need to pace to frontier.
  - Rogue agents can't be controlled as the incident with HuggingFace illustrates (where is the time we were not proud of fuck ups like these.)
  - China is stealing Anthropics IP by distilling their models (I can't even).
  - ...
- All frontier AI companies suffer from huge expenses that they will struggle to pay back. Several failed attempts at price hikes and performance degradations illustrate their desperation. 


This dizzying number of incidents has made me question: How reliable are Anthropic (and its like) as partners for regular people who want use AI for any mission-critical purpose? If Anthropic decides to pull access, use your data, degrade the service, hike prices and you rely on it for the core of your business, what do you do? What will people with such a track record do when they do gain monopoly status?

Here, the solution is technical and adoption. First, Chinese companies like Deepseek, Z.ai and Alibaba have been open-sourcing powerful LLM's trailing the frontier by 3-6 months and closing the gap fast. They are already competitive on most tasks companies may care about and only lag behind a bit when tasks are not yet fully specified, such as in system designs, intent specification and so on. Second, an ecosystem of open-source tools such as code harnesses, LLM servers, AI gateways, chat interfaces etc has been developed at lightning speed, allowing you to take advantage of any model with a compatible API. Third, the availability of open-source models has allowed cloud services around the world to serve these models at low price in servers near you. Open-source AI endpoints from Scaleway, Mistral and OVH cloud allow European developers to use these models without fearing a government mandated cut-off. While an American company, Databricks has open-sourced its core technologies (Unity Catalog, Spark, Delta, ...) and built its platform around customer choice between frontier and open-source models on a well-integrated data platform if you need that convenience. In other words, there are tons of options, but it requires people to not fall for the convenience trap of completely outsourcing AI and to have the information they need.


# The pursuit of meaning: How can we orient our careers towards meaningful endeavours in the AI age?
In the context I have just described, it is no wonder that the public generally hates AI. For most people and young people in particular, AI means:
- slop on LinkedIn.
- a constant threat to their livelihood, which is extra real for young people
- workflow changes that sap the joy from their work
- documented fatigue and exhaustion
- eyewatering piles of money burned only to make real problems like climate change worse and they are coming for our pensions next.

All this and they don't get to see any benefits in return nor do they have any say into where this technology is going. It is no wonder that depression and burnout are up. 

In my view, the fact that AI can cause such a multitude of problems speaks to how powerful it is as a technology. I hold out hope that this technology, when properly harnessed, can do good in proportion to the bad that we are seeing right now. "When properly harnessed" of course does a lot of work, since it forces us to consider what that means. Every build we do, is a choice we make. This is a responsibility I don't feel ready for, but I feel the need to come up with some framework for the conditions that would make an AI application a force for good.
First, AI development must be driven by problems. Whose problems we choose to care about, will determine our impact on the world. It is very different to care about making profit than it is to care about saving lives to put things comically stark.
Second, our solutions must address systemic problems. While I respect the grind, I don't believe that AI applications that generate realistic looking cat pictures will change the world meaningfully. For me, AI should be deployed to solve important problems in health, climate, energy, food, ... as first priorities and not as reputational green-washing. 
Finally, the methods we use should reflect what we want to achieve. I don't believe we will be succesful in keeping AI under control if we keep using frontier models for everything and hoarding our data. In this field, I'm particularly excited by the aforementioned developments in open-source but also the growing number of data spaces, which provide a market place to make the sharing and trading of data in a fair way possible and to the benefit of all. This community building of capability and power is one of the best answers we have against some of these growing threats.


# How I want to contribute to adressing these problems
The purpose of this website is to share information that helps addressing these grand challenges.
To that end, the website consist of three parts:
- Engineering: These will contain blogs about the technical aspects of data and AI engineering, with a particular emphasis on open-source technologies and their leaders. I want to be both pragmatic and acknowledge where proprietary solutions make sense as well as resolute in the commitment to consumer choice and independence.
- Thinking: These will contain blogs in which I reflect on the implications what I do and that they remain aligned with my north stars. I don't want to promise that I will never violate my commitments or change my mind, but I hope these can help the difficult conversations we find ourselves in.
- Products: These will contain the fruits of my labours as an AI product builder. Here, I want to make concrete what meaningful goals mean as well as figure out myself how to navigate the new workplace. At the time of writing, I'm working on a data product pretotype that should make it easier to connect patients of rare diseases to trial, therapeutic and scientific opportunities.

Additionally, I'm offering paid teaching opportunities on any of the topics if you need me to deepen some of these thoughts out more. I'm also very eager to hearing from you what you want me to write about next.



# AI critique
Big picture
The "why" comes too late. The title promises your reasons, but about 80% of the post is a survey of AI's problems. Your own stake only shows up at the very end: the rare-disease pretotype, the builder role, the teaching. Move a short version of that to the top ("I'm building a tool to connect rare-disease patients to trials, and doing so made me confront three problems…") and the crises become your motivation instead of a news roundup.
Your strongest argument is buried. You use Fable to make the power-concentration point through the biology guardrails. The export ban is the much better example for a European reader. A foreign government switched off a model worldwide three days after launch. That is exactly the "government-mandated cut-off" you set up Scaleway and OVH as a hedge against. The guardrails are about safety tuning, which is a weaker example of power concentration.
Real incidents are mixed with sarcasm. Placing "doomer marketing" next to the Hugging Face breach undercuts you, because that breach did real damage. Asides like "I can't even" and the profanity make readers trust your factual claims less, and those claims are mostly right.
Line 31 contradicts your site. "Only by doing and feeling, not thinking" sits oddly on a site with a section called Thinking.
Line 34 is left hanging. "The impact of AI on work is in my opinion understated" either needs an argument or should go.
Fact-check by section
Workforce (Meta / Project OT)
Mostly confirmed. A few details need fixing:

Numbers: right. Plans for teams up to 60% smaller, a 10% layoff in May, the November round cancelled, and a 40% rise in incidents. Say "major technical and security incidents, year over year." Time spent handling them also rose 70%, which strengthens your point. Sources: Pragmatic Engineer, Portada, TechJack on the telemetry.
"All employees had mouse-tracking installed" is too broad. It was US employees, under the "Model Capability Initiative." The "coding puzzles" were engineers (20–30% in some orgs) reassigned to data labelling. Sources: Engadget, Cybernews.
"Unionization… clearly effective in Meta's case" is overstated. The US pushback was flyers and an NLRA petition, not a union. Formal organising was in the UK, with UTAW (Slashdot/Bloomberg summary, Cryptopolitan on Meta's retreat). The poor internal results also contributed. Say "collective pushback", not "unionization."
"Immense reputational damage" has no source. Cut it or cite one.
The Anthropic comparison may cut against you. Summaries of Anthropic's team structure describe two-pizza teams that keep a PM and a designer, with PMs described as more important (eng-leadership newsletter, Claude blog). That doesn't match OT's merged "builder" role. Read the original before you write "note the similarities." Also add the missing link.
Suggested support for "understated": Stanford's Canaries, Aug 2026 update finds employment of 22–25-year-olds in AI-exposed jobs is 19% below trend. But it also finds no widespread displacement. That supports "it hits young people first" better than "understated."
Power concentration
Pentagon refusal: confirmed. See NPR and Axios.
Mythos: confirmed, but your framing is off. Access was restricted because of its zero-day exploit capability, not a general "too powerful for mortals" (InfoQ, NBC).
Fable export ban: confirmed, with two caveats. It was pulled three days after the June 12 launch, but the controls were lifted on June 30 (Forbes, CNBC). As written, readers will think it's still banned. The ban also followed reports of a jailbreak, which somewhat weakens "guardrails clearly overshot."
Bio guardrails overshooting: well supported. See The Register and the arXiv study on biomedical refusals. But Anthropic cut biology false positives by about 85% in August (Anthropic). Say so, or you'll look out of date.
Performance manipulation: fine as "accused." Anthropic: Fortune, where Anthropic denies it. OpenAI: GPT-5.5 routing reports, which mention class actions. Find a stronger outlet than KuCoin for the OpenAI part.
Navier-Stokes: confirmed as an accusation. OpenAI disputes it. See Cybernews, Forbes and Wikipedia.
Bio lab "directly controlled by Claude" is wrong. Human scientists do the bench work. Claude designs and directs, the lab is BSL-1/2 only, and full autonomy is a stated future goal (TechCrunch, Anthropic, Gizmodo). Your real point still stands: the lab keeps biology capability in-house while restricting what users can ask. Gizmodo makes exactly that argument.
Hugging Face: you've misattributed it and misframed it. It was an OpenAI incident, it caused real damage (about a third of HF's infrastructure had to be rebuilt), and it's not marketing (Wikipedia, OpenAI). The Bulletin piece "Rogue AI didn't breach Hugging Face, human decisions did" makes the argument you seem to want, and makes it better.
Distillation: say what's wrong with it instead of "I can't even." The claim is 24,000 fraudulent accounts and 16M exchanges (CNBC, TechCrunch follow-up, Sept). If you think it's hypocritical, argue it. Anthropic's own training-data record is an obvious angle: look up the Bartz v. Anthropic settlement.
"All frontier companies struggle to pay back costs" is partly refuted. OpenAI projects a $14B loss for 2026 (ValueAdd). Anthropic reported a profitable Q2 2026 (Forbes). Prices are currently falling in a price war (Fortune, Sep 22). "Failed price hikes" is fair for Anthropic's Claude Code/Pro test, which it walked back (Boston Globe). "Desperation" overreaches.
Open-source alternatives
The gap is not closing fast. Epoch puts it at about 4 months since January 2026 and says it has widened slightly (Epoch). "3–6 months" is fine; "closing the gap fast" is not.
Leaning on Chinese open models has a tension. You're arguing for independence from one government while relying on another country's labs. Those labs are also the ones accused of distillation. Name the tension, and give Mistral more weight as a European model maker, not only a host.
European hosting is well supported. See Scaleway Generative APIs, Mistral regional inference and the EdenAI comparison of OVH and Scaleway.
Databricks needs one caveat. The open-source Unity Catalog is not the same product as the managed one. Readers who know this will notice.
Meaning
"The public generally hates AI" is overstated. Pew found 52% more concerned than excited and 37% equally concerned and excited (Pew). Your point about young people is well supported: 55% of under-30s are more concerned than excited (Pew, Aug). Since you're in Europe, also see Pew's global youth survey.
Fatigue is supported: HBR, "When Using AI Leads to Brain Fry". The same study finds AI reduces burnout when it takes over repetitive tasks, which actually supports your "harness it properly" section.
"Depression and burnout are up" has no source, and it implies AI is the cause. Source it or cut it.
Climate: the claim is real but modest globally. Data centres are about 1.5% of global electricity. Their emissions reach about 2% of power-sector emissions by 2035 (IEA). The local grid strain is a stronger argument than "makes climate change worse."
Pensions: supported. See PitchBook and Quartz on 401(k) exposure.
Data spaces: be honest about their state. Most are still struggling to become self-sustaining after their pilot funding (data.europa.eu). Saying so makes your optimism more credible.
"Cat pictures" is a strawman. Your framework is good without it.
Typos
"pace to frontier" → race to the frontier
"Anthropics IP" → Anthropic's
"LLM's" → LLMs
"Where am we" → Where are we
"succesful", "adressing" → successful, addressing
"the website consist" → consists
"helps addressing" → helps address
"eager to hearing" → eager to hear
"deepen some of these thoughts out" → pick "deepen" or "flesh out"
"clubbing developer…" → merging
"Anthropic, OpenAI and the new AI startups" repeats "new AI startups"
The line about paid teaching lands abruptly in a mission post. Consider moving it to your About page.
Most important fixes
Put your personal reason first.
Make the export ban your main power-concentration example.
Correct the claims about the bio lab, Hugging Face, unions and "closing the gap fast."
Add "later lifted" and "later relaxed" wherever something has changed since.


Overall tone
The post reads like a disillusioned insider venting, then switching to hope in the last quarter. The hope is the most convincing part, but the reader has to get through a lot of anger to reach it. By then they may have filed you as "another AI cynic," which is the opposite of what an AI builder offering teaching wants.

The register also keeps changing. You move from "completely bummed out" to "generational level challenge" to "(I can't even)" to "resolute in the commitment to consumer choice." Choose one voice. I'd go with reflective and personal, with dry wit used rarely. That fits your Thinking section and your humility, which is the best part of your voice.

What works (keep these)
Your humility: "I only have partial answers", "a responsibility I don't feel ready for", "I don't want to promise that I will never violate my commitments." These lines make you trustworthy.
"Your coworkers are your friends." It's short and warm, and it says something.
"Every build we do is a choice we make." This could be the thesis of the whole site.
Your three conditions for good AI (problem-driven, systemic, methods that match the goals). This is original thinking. Everything else in the post is commentary on news.
What hurts the tone
The snarky parentheticals. "(although using it for other warfare purposes is fair game I guess)", "(where is the time we were not proud of fuck ups like these.)", "(I can't even)", "but I digress." Each one is small, but together they sound sneering. A reader who disagrees stops reading, and one who agrees learns nothing new. Remove all of them. If a point is worth making, make it in a full sentence.
The grievance list. The long bullet list of bad practices reads like a rant because it has no ranking and no argument. Several items don't support your actual question, "can I rely on this vendor?":
Navier-Stokes and distillation are about intellectual property and ethics.
Hugging Face is about safety.
Grandiosity. Your problems come "on top of wars, famine and climate change." Then comes "I want to address them head-on here." A personal website can't do that, and the gap between the claim and what the site can do makes it sound naive. Promise something smaller: "share what I learn while trying to build responsibly."
Everything is aimed at Anthropic and OpenAI. Google, Microsoft and Meta's AI products barely appear. The post reads less like a structural critique and more like a disappointed former fan. Either broaden the examples or say openly that you're a heavy Claude user and that's why this section hurts. Saying it would make the post more personal.
The dates will go stale. "A couple of months ago" and "since then" won't mean anything in a year. Use months: "In June 2026…"
What I'd add
You. Who are you, what do you build, and why rare diseases? Right now the reader learns more about Zuckerberg than about the author. Include any personal connection to rare diseases only if you want to share it. Even "I'm a data engineer who…" would help.
One experience of your own with AI at work. You argue that these questions can only be settled by doing, but the only thing you've done in the post is use Fable for 2–3 days. One specific example (it made you faster here, drained you there) would support the whole workforce section.
Evidence for the hope. Section 3 says AI can do good but gives no example. The irony is that Anthropic's enzyme discovery is exactly that kind of example, from the company you criticise. Holding both ideas at once ("this worries me and this is why I build") is more interesting than either one alone.
Who the site is for. Engineers? Managers facing a Project OT of their own? Patients? One sentence is enough.
A real ending. The post ends on a sales pitch for teaching. End on your thesis instead, then the call to action: RSS or subscribe, "tell me what to write about," and teaching last, or on your About page.
What I'd remove
All the snarky parentheticals.
The bad-practices list, cut to the 2–3 items that show dependency risk: export ban, silent degradation, price and plan changes. Save the rest for a later Thinking post.
"wars, famine and climate change."
Line 34 ("The impact of AI on work is in my opinion understated"), which is left hanging.
The section headings that repeat the intro questions word for word. Use short headings like "Work", "Power", "Meaning".
The # H1 headings. Your Jekyll theme probably already renders the post title as the H1, so use ##.
Structural suggestion: split it into two posts
This is two posts wearing one title:

"Why I'm building this website": about 600 words. It covers you, the rare-disease product, the three crises as one paragraph each, your three conditions, and what the site will contain.
"The three crises of AI": the full essay with sources, linked from the first post.
The short post is what new visitors will read. The long one is what they'll share.

An example opener
This is only to show the register, with placeholders where your own facts go:

I build data and AI products for a living, and right now I'm working on something that matters to me: a tool to help people with rare diseases find trials and researchers. Doing that work has forced me to face three questions I can't avoid as a builder — what AI is doing to our jobs, who ends up controlling it, and how to point it at things that matter. This site is where I work through them in public.

It's calm, first-person and specific, and it would carry the rest of the post.