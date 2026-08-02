---
title: "How and Why I Write Data Product Definition Documents"
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
excerpt: "How I think about documenting data products at the beginning of a project"
---

## Introduction
How to document software projects, including data projects, is a hotly
debated subject with as many opinions as there are people.
The goal of this blog is then simple: make my personal thinking about 
data product documentation transparent. This has two benefits in my opinion:
- Readers of this blog will understand better why I document things
  the way I do and be better positioned to critique it.
- I force myself to articulate and better understand my own reasoning.

This blog will focus on what I call a data product definition document.
These documents are primarily used to communicate a shared understanding
of what needs to be built and contain the information needed to start a data
product development project. I consider them the precursor of a more traditional
product requirements document.

I'm getting ahead of myself though. Let's first start by why I'm talking about
data products.

## Data as a software product
From what I understand, the idea of considering data as a product originates
from data architectural discussions. Zhamak Dehghani seems to have coined
the term data mesh [^1]. Briefly, a data mesh is an organizational model
in which central IT teams define governance rules (e.g. how to name things, what information is required for regulatory purposes, ...) and enforcement,
but decentralized data product teams are responsible for the actual maintenance of data.
In this way of thinking, a data product is more than just a table. It is a reuseable asset
that not only contains the data, but several other requirements to make sure that other people can trust and use it (e.g. column descriptions, data quality tests, ...). 

In my opinion, treating data as a product that has its own requirements has the following benefits:
- It encourages good design thinking. A well-designed data product is infinitely more useful than a random table with columns no one understands.
- It surfaces that engineering reliable data is about more than making technically correct data pipelines. Especially useful in discussions with  outside stakeholders.
- It encourages active lifecycle management. Products that have served their purposes are decommissioned. Data products should be the same as they require an ongoing maintenance cost.
- It makes it easy to assign ownership and responsibility. If something is wrong, talk to the data product owner.

Treating data as a reusable product asset in my view has several practical upsides, but why should you document them in the first place?

## The purpose of data product documentation
What to document and what not is one of these endless discussions in the field.
This is because of the tension between overdocumenting and underdocumenting:
- The cost of overdocumenting is clear. If you write a document that adds no value,
  you not only have wasted your time now, but also for the future you that needs to maintain that
  document. That's the Agile Manifesto's whole argument for working software over comprehensive documentation: 
  excess documentation doesn't just cost time to write, it costs ongoing effort to keep honest and
  it turns from an asset into a liability.[^2]

- The cost of underdocumenting is the creation of technical debt [^3].Technical debt is
  accumulated gap between what you actually understood at the time of writing code and what you know about it now.
  It is considered debt, because the effort to relearn what a dataset is about or a piece of code does is always greater
  than just writing down your key understandings in the moment.
  
There is also the problem of over- and underdocumenting at the wrong stage of the project.
Data product requirements are necessary at some point during the project, but trying to get them before exploring your data is just a waste of time.

In this delicate balance I use the following principle: **a data product should have the documentation that lets it move smoothly into its next phase, and not one document more or less**.

When you start a data project, I think it needs a data product definition document. 

## The purpose of a data product definition document

In my view, the data product definition document is a communication tool.
Its purpose is to create a shared understanding around the data product within the development team and with outside stakeholders.
It should answer the following questions, adapted from Amazon's working-backwards PR/FAQ questions [^4]:
- Who benefits how from having this data product? **value**
- To the best of our knowledge now, can we realistically do this? **feasibility**
- Given our limited resources, when should we do what? **prioritization**

The value question is really important to nail down before starting any work.
For each data product, it should be clear how many people or cases are affected,
what the cost is of not having the data product for these people (time, money, harm).
A document without those numbers is an opinion, a document with them is a case.
Defining value fairly is really difficult in general. In an industrial context, it requires ending up with a money figure saved or made.
For these open-source projects, I relax this standard to having documented proof that people who are suffering are not getting enough help.

Feasibility is difficult to assess at the start of a data project, so I consider it feasible if the data needed to make a specific product **exists**.
In the data exploration phase, I usually uncover many problems that require me to update my beliefs. I consider feasibility an evolving thing, not something I want to pin myself down on.


Solving the value question and the feasibility question strongly informs the prioritization discussion.
With that input and the right prioritization framework, it should be clear what is worth spending time on.


## The contents of a data product definition document
The actual data product definition document should translate the above philosphy into concrete artifacts.
Here is how mine does that?
**Vision.** What does this data product make possible that wasn't possible
before:  grounded in the actual numbers from the value question, not
aspiration. If you can't back the vision with a real count of who's affected
and a real cost for the status quo, you don't understand the value well
enough to write anything below it.

**Epics.** The work grouped by the value it delivers, not by the technical
layer it touches. An epic is "patients get matched to trials automatically,"
not "build the join between two tables".

**User stories.** Each epic breaks into a handful of stories, in the "as a X,
I want Y, so that Z" shape. The "so that" clause is the one that matters —
it's what keeps a story traceable back to the value question, instead of
drifting toward describing the pipeline.

**Success criteria.** How do we know the whole thing was worth building,
once it's live. Tied directly back to the numbers in the vision: not
"patients get matched faster" but "time from a confirmed variant to a ranked
trial list drops from days to a query."

**Per-epic success criteria.** The same question, asked at a smaller grain:
what does "this one epic actually delivered its value" mean, concretely
enough that nobody has to guess. Whole-product and per-epic success criteria
are the same question (how will we know this was worth building )answered at two different zoom
levels. 

**Risks and assumptions.** Whatever the rest of the document is 
resting on that isn't yet verified (a technical unknown, a dependency on
someone else's decision, an assumption about which population is actually
addressable).[^5] Even if you decide that something is feasible, it is worth documenting
on what grounds and assumptions, because you may need to revisit this throughout the project

**Prioritization.** What's most important to do, and when. What I actually use is three factors: **value, effort and dependencies**. 
Value-versus-effort is a standard lens on its own,[^6] and its most commonly documented weakness is
that it ignores exactly the third thing I want: sequencing and
dependencies between items.[^6] It directly answers an outside reader's real question (what's
realistic soon versus later) rather than producing a score that only means
something inside a planning meeting. Prioritization is not rocket science: a rough low/medium/high is enough. The
discipline is in being forced to state a value, an effort, and a dependency for every item.

Every one of these traces back to one of the three questions from the
section above: vision and both success-criteria levels answer value; risks
and assumptions answer feasibility; epics, stories, and prioritization
together answer what matters most and when.

## Conclusion

This post exists to make the thinking behind a data product definition
document transparent: why I write one at all, what goes in it and why everything is there.
It discussed the importance of treating data as a product with requirements beyond just being correct.
It explained the tension between overdocumenting and underdocumenting data products and established a firm principle: no more and no less documentation then is needed to move the project forward at this stage.
It argued that a data product definition document should answer the value, feasibility and prioritization questions and not provide comprehensive requirements.
It borrowed concrete tools from Agile practices to create artifacts that answer just that.
This philosophy post (and future ones) will help readers understand my reasoning for how I work, invite helpful critique and maybe also help others reason through these problems for their own projects.

[^1]: Zhamak Dehghani (2019). [How to Move Beyond a Monolithic Data Lake to a Distributed Data Mesh](https://martinfowler.com/articles/data-monolith-to-mesh.html), martinfowler.com — the "data as a product" principle: discoverable, addressable, trustworthy, self-describing, interoperable, secure.
[^2]: [12 Principles Behind the Agile Manifesto](https://agilealliance.org/agile101/12-principles-behind-the-agile-manifesto/), Agile Alliance.
[^3]: [Introduction to the Technical Debt Concept](https://agilealliance.org/introduction-to-the-technical-debt-concept/), Agile Alliance — Ward Cunningham's original 1992 metaphor, applied here to undocumented reasoning rather than unrefactored code.
[^4]: [The Working Backwards PR/FAQ Process](https://workingbackwards.com/concepts/working-backwards-pr-faq-process/) — the process behind Amazon Prime, Kindle, and AWS, among others.
[^5]: A RAID log ([Risks, Assumptions, Issues, Dependencies](https://asana.com/resources/raid-log)) is the standard project-management version of this same log; a data product definition document only needs the risks and assumptions half of it.
[^6]: [Prioritization frameworks](https://www.atlassian.com/agile/product-management/prioritization-framework), Atlassian — the value-vs-effort matrix and its documented tendency to ignore urgency, dependencies, and sequencing.

