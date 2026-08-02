---
title: "How and Why I Write Data Product Requirements Docs"
date: 2026-07-31 10:30:00 +0200
categories: [primer]
all_projects: true
audience:
  - data-engineers
  - bio-it-leaders
tags:
  - product-management
  - agile
  - data-mesh
excerpt: "Why I treat data as a product, why a PRD exists to communicate benefit, feasibility, and priority to outside readers rather than to organize work, and why I prioritize by value, effort, and dependencies instead of MoSCoW or RICE."
---

Four questions this is built around: why treat data as a product at all, what
a PRD is actually for, what goes into one, and how I decide what to build
first.

## Why I view data as a reusable product

I treat every dataset, table, or model a project publishes as a product, not
a byproduct of some other pipeline. That's not just a metaphor — it's
Zhamak Dehghani's actual argument for data mesh: a data product should be
discoverable, addressable, trustworthy, self-describing, interoperable, and
secure — the same properties you'd expect from anything built for someone
else to depend on.[^1]

What I take from that, specifically: framing data as a product forces me to
ask who the end user actually is and what they need, not just what's
technically convenient to expose. It also forces a decision that's easy to
duck otherwise — someone has to own it. That framing is comprehensive enough
to cover discoverability, trust, and interoperability, but still simple
enough to put one name on the thing and one person on "who's responsible when
this breaks."

I won't lay out the full checklist of what makes a data product good here —
that's a post of its own, coming later. This one is about the document that
decides whether a data product is worth building in the first place.

## What is a PRD for me?

I work alone. There's no internal team here for a PRD to align, no delivery
workflow for it to organize, no ceremony for it to feed. So this document
isn't there to organize work — it's there to communicate with people outside
my own head: the people I hope will actually use or benefit from what gets
built, who need to understand it well enough to react to it.

Everything in it has to answer one of three questions on that outside
reader's mind, adapted from Amazon's working-backwards PR/FAQ questions —
who is this for, what changes for them, how do we know we've
succeeded[^2] — down to three: **does this benefit people, is it actually
possible, and what's most important to do, and when.** If a piece of the
document doesn't answer one of those three, it doesn't belong, regardless of
how interesting it is to write.

Benefit gets the most depth of the three. How many people or cases this
actually touches, and what it costs — in time, money, or harm — for the
problem to stay unsolved today. A PRD without that number is an opinion; a
PRD with it is a case. Everything about how the thing gets built stays
deliberately shallow by comparison — that's not this document's job.

That's related to, but distinct from, a second reason a PRD isn't a full
spec: the Agile Manifesto's founding preference for working software over
comprehensive documentation, and for welcoming changing requirements even
late, holds regardless of team size — it would still be true if I had ten
engineers.[^3] A PRD is a living document: the best shared understanding of a
data product's value at the time it's written, not a contract that stops
being true the moment something is learned during the build. My reason is
additional to that one, not a restatement of it: even a perfectly Agile team
of ten would still need a document like this to talk to the people outside
it, which is the part that's specific to working the way I do.

## What does a PRD contain, and why?

**Vision.** What does this data product make possible that wasn't possible
before — grounded in the actual numbers from the benefit question above, not
aspiration. If you can't back the vision with a real count of who's affected
and a real cost for the status quo, you don't understand the value well
enough yet to write anything below it.

**Epics.** The work grouped by the value it delivers, not by the technical
layer it touches. An epic is "patients get matched to trials automatically,"
not "build the join between two tables" — the second is real work, but it
answers a question this document isn't for.

**User stories.** Each epic breaks into a handful of stories, in the "as a X,
I want Y, so that Z" shape. The "so that" clause is the one that matters —
it's what keeps a story traceable back to the benefit question, instead of
drifting toward describing the pipeline.

**Success criteria.** How do we know the whole thing was worth building, once
it's live — stated before the epics, not after, because a list of epics only
means something once you already know what would prove them worth having
built. Tied directly back to the numbers in the vision: not "patients get
matched faster" but "time from a confirmed variant to a ranked trial list
drops from days to a query."

**Per-epic success criteria.** The same question, asked at a smaller grain:
what does "this one epic actually delivered its value" mean, concretely
enough that nobody has to guess. Whole-product and per-epic success criteria
are the same question — was this worth it — answered at two different zoom
levels. Neither is an engineering checklist; both still answer benefit, not
"is this piece technically finished."

**Risks and assumptions.** Whatever the rest of the document is quietly
resting on that isn't yet verified — a technical unknown, a dependency on
someone else's decision, an assumption about which population is actually
addressable.[^4] This is the feasibility question, made explicit and
revisited as it resolves, rather than left as an implicit hope until one of
them turns out to be wrong.

Every one of these traces back to one of the three questions from the
section above: vision and both success-criteria levels answer benefit; risks
and assumptions answer feasibility; epics, stories, and prioritization
together answer what matters most, and when.

## How do I prioritize?

I don't use MoSCoW or RICE. Both exist to negotiate priority among multiple
stakeholders or teams — a coordination problem I don't have internally — and
neither actually helps the one audience this document has, an outside
reader, understand what matters and why. On top of that, MoSCoW's
Must/Should/Could/Won't buckets end up feeling arbitrary past the first
pass, and RICE's Reach and Confidence are usually guesses before anything has
shipped — a precise-looking formula built on guesses isn't better than an
honest rough call, it's worse, because it looks more certain than it is.

What I actually use is three factors held side by side rather than combined
into one score: **value**, **effort**, and **dependencies**. Value-versus-
effort is a standard lens on its own,[^5] and its most commonly documented
weakness is that it ignores exactly the third thing I want in the room:
sequencing and dependencies between items.[^5] Even a framework built to be
rigorous about this — SAFe's Weighted Shortest Job First, which divides cost
of delay by job size[^6] — doesn't fold dependencies into that formula
either; it handles them separately, mapped visually on a program board during
planning.[^7] That's a good sign dependencies deserve their own explicit
column rather than being squeezed into a formula that was never built to
hold them. Effort and dependency information also does something MoSCoW and
RICE don't: it directly answers an outside reader's real question — what's
realistic soon versus later — rather than producing a score that only means
something inside a planning meeting.

The number I put on each of the three doesn't need to survive an audit — a
rough low/medium/high is enough. The discipline is in being forced to state a
value, an effort, and a dependency read for every item, not in the arithmetic
afterward.

[^1]: Zhamak Dehghani (2019). [How to Move Beyond a Monolithic Data Lake to a Distributed Data Mesh](https://martinfowler.com/articles/data-monolith-to-mesh.html), martinfowler.com — the "data as a product" principle: discoverable, addressable, trustworthy, self-describing, interoperable, secure.
[^2]: [The Working Backwards PR/FAQ Process](https://workingbackwards.com/concepts/working-backwards-pr-faq-process/) — the process behind Amazon Prime, Kindle, and AWS, among others.
[^3]: [12 Principles Behind the Agile Manifesto](https://agilealliance.org/agile101/12-principles-behind-the-agile-manifesto/), Agile Alliance.
[^4]: A RAID log ([Risks, Assumptions, Issues, Dependencies](https://asana.com/resources/raid-log)) is the standard project-management version of this same log; a PRD only needs the risks and assumptions half of it.
[^5]: [Prioritization frameworks](https://www.atlassian.com/agile/product-management/prioritization-framework), Atlassian — the value-vs-effort matrix and its documented tendency to ignore urgency, dependencies, and sequencing.
[^6]: [Weighted Shortest Job First (WSJF)](https://framework.scaledagile.com/wsjf), Scaled Agile Framework — cost of delay divided by job duration.
[^7]: [PI Planning](https://framework.scaledagile.com/pi-planning/), Scaled Agile Framework — cross-team dependencies mapped on the program board, separately from any prioritization score.
