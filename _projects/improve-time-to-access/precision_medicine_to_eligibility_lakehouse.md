---
title: "DMD Eligibility Lakehouse"
excerpt: "A governed data platform linking genetic mutations in DMD to the therapies and trials a patient is eligible for."
date: 2026-07-31
stage: pretotype
mission: improve-time-to-access
outcome: "Aims to replace a week of manual registry cross-referencing with a single query."
outcome_measured: false
role: "Solo"
timeline: "Started Jul 2026"
stack:
  - Databricks
  - OMOP CDM
  - Delta Live Tables
header:
  teaser: /assets/images/projects/precision-medicine-eligibility-teaser.jpg
tags:
  - databricks
  - rare-disease
  - precision-medicine
  - lakehouse
---

<div class="hero-diagram-wrap" markdown="1">
![Diagram of dystrophin connecting intracellular actin to the extracellular matrix](/assets/images/projects/precision-medicine-eligibility-teaser.jpg)
</div>
*Dystrophin diagram by Daniel E. Michele and Kevin P. Campbell, via [Wikimedia Commons](https://commons.wikimedia.org/wiki/File:Dystrophin_diagram.jpg), [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) — the protein this whole eligibility model is ultimately about.*
{: .img-caption}

<section class="proj-section" markdown="1">

## Problem

The four FDA-approved exon-skipping therapies for DMD cover only 27% of patients by mutation alone. The other 73% have real options, but identifying which one a specific patient qualifies for means manually cross-referencing mutation registries, the reading-frame rule, and free-text trial eligibility criteria across systems that don't talk to each other — a process that doesn't scale across a registry and doesn't update when a trial opens or closes.

<a class="artifact-link" href="{{ '/concepts/duchenne-muscular-dystrophy/' | relative_url }}"><span>📖</span> New to DMD? → What is Duchenne muscular dystrophy?</a>
<a class="artifact-link" href="#"><span>📄</span> Deep dive → <code>docs/business_case.md</code></a>

</section>

<section class="proj-section" markdown="1">

## Proposed solution

Before building this lakehouse, I want to know whether the data allows it and whether anyone needs it. Today, finding that out takes a lot of expert work, all paid before there is any sign of demand. So I'm building four tools, each making one step cheaper:

1. **Automated data discovery.** Starts from a question and ends with a set of datasets: how feasible each one is from different perspectives, such as access, licence, quality and privacy, and how they join together.
2. **Automated pretotyping.** Takes those datasets and their constraints, invents synthetic data where it has to, and proposes data product ideas to test demand with.
3. **Automated prototyping.** Turns a selected pretotype into a small version that actually works, on Databricks, GCP or another platform.
4. **Automated build.** Takes a prototype through everything a regulated environment needs, such as tests, lineage and data contracts.

DMD is the first test case: this lakehouse becomes one possible outcome, and goes through the same go/no-go as any other idea. Data discovery is in design now. The other three come later.

</section>
