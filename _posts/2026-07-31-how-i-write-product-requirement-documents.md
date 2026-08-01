---
title: "How I Write Product Requirement Documents"
date: 2026-07-31 10:30:00 +0200
categories: [primer]
all_projects: true
audience:
  - data-engineers
  - bio-it-leaders
tags:
  - product-management
  - agile
  - project-definition
excerpt: "What a PRD is for, why it fell out of fashion, what Agile's own toolkit replaced it with, and how to write one that actually gets used."
---

Every [project definition post]({{ "/projects/" | relative_url }}) on this
site is, structurally, a small PRD — a product requirements definition. This
primer is the "why" and "how" behind that choice, for anyone who hasn't had
to write one before.

## What a PRD is for

A PRD's job is narrow: get everyone who touches a project — engineers,
stakeholders, the person funding it, future-you six months from now — aligned
on the same understanding of *what problem is being solved, for whom, and
what "done" means*, before anyone commits real engineering time to it. It is
not a technical spec. It is not a project plan. It's the argument for why the
work is worth doing, written down so it can be checked later against what
actually got built.

## Why PRDs got a bad reputation

Marty Cagan, who wrote one of the most widely cited early guides to writing a
good PRD, spent much of the following decade walking that advice back. His
core complaint wasn't the document format — it was what teams used it *for*:
"If you think you can get what you need by having product managers document
PRDs instead of product discovery, then you may as well just give up on
innovation."[^1] The failure mode is specific: a PRD written in isolation,
handed down as a finished spec, that substitutes for the messier work of
actually validating the problem with real users and real constraints before
committing to a solution. A document can't do discovery for you — it can only
record what discovery already found.

The Agile movement's answer was to mostly do away with heavy specs
altogether. Its founding principles put "working software over comprehensive
documentation" first, prioritize satisfying the customer through early and
continuous delivery, and treat welcoming changing requirements — even late —
as a competitive advantage rather than a failure of planning.[^2] In practice
that becomes lighter, living artifacts (user stories, backlogs) that stay
close to the code, revised every iteration instead of approved once and
filed away. That's the right instinct for a feature inside an existing
product. It's a worse fit for the question this site's project-definition
posts are actually trying to answer, which is closer to "should this project
exist at all" than "how should this ticket be implemented."

## The format that survived: working backwards

Amazon's answer to the same problem is the PR/FAQ: before building anything,
write the future press release announcing the finished product, plus the FAQ
that would follow it — from the customer's perspective, not the
implementation's.[^3] The discipline it forces is exactly the one a good
project-definition post needs: you cannot write a convincing press release
for a product that doesn't actually solve anyone's problem, no matter how
technically interesting it is to build. Structurally, that's a narrative
version of the same three questions: who is this for, what changes for them,
and how would we know we succeeded.

## The Agile-native toolkit

Where a traditional PRD tends to bundle everything — problem, scope,
priority, success criteria — into one prose document, Agile practice splits
those jobs across smaller, purpose-built tools instead of one artifact:

- **Epics and user stories**, checked against Bill Wake's INVEST
  criteria — Independent, Negotiable, Valuable, Estimable, Small,
  Testable[^4] — replace a monolithic feature list with units small enough to
  actually estimate and ship one at a time.
- **MoSCoW** (Must / Should / Could / Won't have)[^5] replaces a single
  "goals" section with an explicit, negotiated priority order — its Won't
  category does the same job a PRD's non-goals section does, just phrased as
  a decision instead of an afterthought.
- **RICE** (Reach × Impact × Confidence ÷ Effort)[^6] replaces gut-feel
  prioritization with a comparable score across competing ideas — useful once
  there's real usage data to estimate Reach and Confidence from; before that,
  the honesty of MoSCoW beats the false precision of a RICE score built on
  guesses.

None of this makes the PRD's underlying questions go away — value, scope,
priority, done-ness are all still being answered. It just answers them
piece by piece, in artifacts that stay easy to revise, instead of in one
document that's easy to write once and hard to keep honest.

## What makes one worth reading

Stripped of formatting advice, the practices that consistently show up across
modern guidance boil down to a short list:[^7][^8]

- **Concrete over vague.** "Fast" and "user-friendly" mean nothing testable.
  A number, a before/after, or a specific failure mode does.
- **Explicit non-goals.** What you are *not* building is as load-bearing as
  what you are — it's the difference between a scoped project and one that
  quietly grows to cover everything adjacent to it.
- **Success criteria stated up front.** If you can't describe what "done"
  looks like before you start, you won't recognize it when you get there.
- **Written to be checked later, not just approved once.** A PRD that only
  has to survive a single review meeting will be vaguer than one that has to
  still make sense against what actually shipped.

## How this shows up on this site

Most project-definition posts on this site follow the narrative shape:
problem, who it hurts, goals, non-goals, what done looks like, open
questions. The [product definition for the DMD eligibility
project]({{ "/project-definition/dmd-mutation-eligibility-product-definition/" | relative_url }})
uses the Agile-native shape instead — epics, value and feasibility per epic,
MoSCoW prioritization — for
the same reason Agile teams reach for it over a monolithic spec: five
competing data products with genuinely different feasibility profiles are
easier to reason about as five small, scored units than as one long argument.
Either shape has to survive the same test: an honest non-goals (or Won't
have) list, and success criteria stated before the goals that depend on them.

[^1]: Marty Cagan (2006). [Revisiting the Product Spec](https://www.svpg.com/revisiting-the-product-spec/) and [Discovery vs. Documentation](https://www.svpg.com/discovery-vs-documentation/), Silicon Valley Product Group.
[^2]: [12 Principles Behind the Agile Manifesto](https://agilealliance.org/agile101/12-principles-behind-the-agile-manifesto/), Agile Alliance.
[^3]: [The Working Backwards PR/FAQ Process](https://workingbackwards.com/concepts/working-backwards-pr-faq-process/) — the process behind Amazon Prime, Kindle, and AWS, among others.
[^4]: Bill Wake (2003). [INVEST in Good Stories, and SMART Tasks](https://xp123.com/invest-in-good-stories-and-smart-tasks/).
[^5]: [What is MoSCoW Prioritization?](https://www.agilebusiness.org/resource/what-is-moscow-prioritization/), Agile Business Consortium (DSDM).
[^6]: [RICE: Simple Prioritization for Product Managers](https://www.intercom.com/blog/rice-simple-prioritization-for-product-managers/), Intercom.
[^7]: [What is a Product Requirements Document (PRD)?](https://www.atlassian.com/agile/product-management/requirements), Atlassian.
[^8]: [The Only PRD Template You Need](https://productschool.com/blog/product-strategy/product-template-requirements-document-prd), Product School.
