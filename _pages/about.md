---
layout: single
title: "About"
permalink: /about/
author_profile: true
toc: true
toc_label: "On this page"
toc_sticky: true
---

My mission is to build the infrastructure that breaks down the data silos costing
organisations money. Let's break that down.


## Who am I?
My name is Steff Horemans.

I studied a master's in biophysics and synthetic biology, then did a PhD in biology. During it, we published a paper on a molecular mechanism linking metabolism to DNA replication, one of many findings showing that a cell's metabolic state drives the behaviour of everything else it does. This was exciting: it could help explain the link between nutrition and cancer onset, and help us engineer cells to produce more of what we want  (small molecules, antibodies, proteins and so on). 

Note: The paper, for the curious: [Pyruvate kinase, a metabolic sensor powering
glycolysis, drives the metabolic control of DNA replication](https://link.springer.com/article/10.1186/s12915-022-01278-3),
*BMC Biology*.

After my PhD, I moved into data. Over six years, I spent two as a Tableau developer, then four running my own data projects end to end (engineering, visualization, optimization modelling etc). I've had successes and failures in that time, and the failures kept coming back to the same two things:

- **Data availability**. The clean, well-structured data my ambitions needed usually wasn't there. That's part of why I now work mainly as a Databricks data engineer,  closer to the source of the problem.
- **Value focus**. Succeeding at a task that brings no benefit is worse than not succeeding at all. Knowing exactly what outcome you're chasing, before you build anything, is what separates the projects that mattered from the ones that didn't. This may seem trivial, but finding something to do that truly matters is not an easy task in today's heavily abstracted world.

I've been fortunate to work with colleagues on projects that have delighted the people we built them for and have given me a lot of personal joy. 
But lately I've also wanted to bring these skills back to problems in the (bio)sciences where lives are at stake. 
This is why this site exists.

[**Download my CV (PDF)**](/assets/cv/cv_steff_horemans.pdf){: .btn .btn--primary}

**Get in touch:** [Email](mailto:{{ site.email }}){% for link in site.author.links %} · [{{ link.label }}]({{ link.url }}){% endfor %} · [Contact page](/contact/)


## Why this mission?

Data silos are everywhere in the (bio)sciences, and their root causes are usually
organisational rather than technical. Incompatible formats, absent standards, and
funders who don't require interoperability are the visible symptoms; the underlying
cause is that no one is rewarded for the work of curating and harmonising data.[^1]
The consequences are concrete and they affect real people. Two examples.

**Rare disease research.** Data stays locked up to protect intellectual property and
funding streams and there's often no compliant way to exchange it. The result is
uncoordinated parallel studies that collect the same data twice while none of them
accumulates the critical mass of evidence needed to de-risk a drug. Patient
populations here are small and dispersed enough that traditional trial design
struggles already.[^2] Every delayed or abandoned programme is a treatment not
reaching patients who often have no alternative and a rapidly degenerating condition.[^2]

**Biomanufacturing.** Different departments run systems that were never designed to
talk to each other. Researchers track samples in a LIMS. Process engineers track
production in an MES. Finance and supply chain run an ERP. Quality runs a QMS.[^3]
Every manual handoff between them is an opportunity for a transcription error, a
delay in an operational decision and time a bioprocess engineer spends reconciling
spreadsheets instead of interpreting results. In poorly integrated environments,
that reconciliation work consumes 20–35% of a knowledge worker's productive time.[^3]
Organisations that do integrate report up to 35% fewer process deviations and batch
release cycles 20–40% faster.[^3] But the harder-to-measure cost is the analysis that
never happens at all, because the people qualified to do it are busy moving data
between systems.

That cost does not just impact big companies. Development and production expenses shape which therapies get
made, what they're priced at and who can afford them. The same holds outside
medicine: the economics of bio-based chemical production determine whether it can
compete with fossil-derived alternatives at all.

Environments like these need people who are both capable technical data engineers
*and* fluent enough in the domain to talk to every one of those stakeholders and work
out what should actually be built.
## What you will find here

Each [project](/projects/) moves through the same four stages, and each stage
produces its own kind of post, shown alongside that project's own page. You can
follow a project end to end or read only the stage that concerns you.

1. **Project definition**: What is broken, who it hurts, and what has to be true
   for the work to have been worth doing. No code. This is where I decide whether
   a problem deserves an implementation at all.
2. **Data exploration**: What the data actually looks like once you open it: the
   standards in play, the fields that are optional in theory and absent in
   practice, the assumptions we make. Notebooks and findings.
3. **Engineering**: Turning that exploration into an asset other people can rely
   on. Modelling, pipelines, tests, lineage, documentation, and the trade-offs
   behind each. 
4. **Retrospective**: What I chose, what it cost and what I would do
   differently. 

Alongside these run [**primers**](/primers/): short explainers of a single concept or system
(what a LIMS is for a data engineer, what column-level lineage is for a
bioinformatician, ...) written for whichever side of the divide it is unfamiliar to.

## Who is this website for?
Everything here comes with an open reference implementation on the
[Projects](/projects/) page. If you are one of the following people, here is
what you can expect to find:

- **Data engineers moving into biotech**: The number of data sources, standards
  and scientific concepts in this field is hostile to newcomers and very little
  is written for you specifically. The project definitions and data exploration
  deep dives are where I work through that.
- **Bioengineers, bioinformaticians and computational biologists**: Data
  engineering practice is not part of any bio-adjacent curriculum I know of, yet
  it decides whether a dataset is valuable long-term. The
  engineering posts take an exploration and turn it into a data asset you can
  trust.
- **Bio-IT and R&D data leaders**: You have to decide what to invest in and who
  to staff it with. These posts should tell you what a given piece of
  infrastructure costs to build and run, and how to distinguish a real problem
  from an expensive one.

That's the gap I want to work in. If you're wrestling with these problems (or think
I've got some part of this wrong), I'd like to hear about it.

[^1]: Channing, G. & Ghosh, A. (2026). [AI for Scientific Discovery is a Social Problem](https://www.sciencedirect.com/science/article/pii/S2666389926000061). *Patterns*.
[^2]: Denton, N. et al. (2021). [Data silos are undermining drug development and failing rare disease patients](https://doi.org/10.1186/s13023-021-01806-4). *Orphanet Journal of Rare Diseases*, 16:161.
[^3]: Lad, R. N. (2026). [Breaking Data Silos in Biotech: Integration Architectures for Scalable GMP Operations](https://doi.org/10.52088/ijesty.v6i3.1852). *International Journal of Engineering, Science and Information Technology*, 6(3), 53–60.