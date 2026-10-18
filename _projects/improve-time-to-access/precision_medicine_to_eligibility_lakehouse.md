---
title: "From Question to Data Product"
excerpt: "AI workflows that take a research question to a working data product, tested first on Duchenne muscular dystrophy trial eligibility."
date: 2026-07-31
stage: pretotype
outcome: "Aims to cut the expert work between a research question and a go/no-go on a data product."
outcome_measured: false
role: "Solo"
timeline: "Started Jul 2026"
stack:
  - LiteLLM
  - OpenCode
  - Scaleway
  - OVHcloud
  - Databricks
header:
  teaser: /assets/images/projects/precision-medicine-eligibility-teaser.jpg
concepts:
  - duchenne-muscular-dystrophy
tags:
  - rare-disease
  - data-discovery
  - data-products
  - ai-engineering
  - databricks
---

<div class="hero-diagram-wrap" markdown="1">
![Diagram of dystrophin connecting intracellular actin to the extracellular matrix](/assets/images/projects/precision-medicine-eligibility-teaser.jpg)
</div>
*Dystrophin diagram by Daniel E. Michele and Kevin P. Campbell, via [Wikimedia Commons](https://commons.wikimedia.org/wiki/File:Dystrophin_diagram.jpg), [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) — the protein this whole eligibility model is ultimately about.*
{: .img-caption}

<section class="proj-section" markdown="1">

## Problem

The four FDA-approved exon-skipping therapies for DMD cover only 27% of patients by mutation alone. The other 73% have options, but identifying which one a specific patient qualifies for means manually cross-referencing mutation registries, the reading-frame rule and free-text trial eligibility criteria across systems. Furthermore, patients can be ineligible for treatments for other reasons such as age, pre-existing AAV antibodies that prevent gene therapy and more. This means it is hard for patients to find out which treatments they are eligible for, hard for trial managers to figure out which patients will respond best to their proposed treatment and hard for research institutions as a whole to find which patients are currently being underserved by the industry. To solve that problem, you would need data products on a data lakehouse that properly combine these disparate data sources together.
Such a DMD eligibility lakehouse generalizes in two ways. 
First, DMD is not the only rare disease where I haven't been able to find a database that explicitly links trial/therapy eligibility criteria to the underlying disease profile that would benefit most. Hence, the final solution should generalize to multiple rare diseases if possible.
Second, the problem itself is general. It is hard to go from an idea to figuring out what datasets are meaningfully available to building a valuable data product. 
Hence, this is the problem we are tackling.

<a class="artifact-link" href="{{ '/concepts/duchenne-muscular-dystrophy/' | relative_url }}"><span>📖</span> New to DMD? → What is Duchenne muscular dystrophy?</a>
<a class="artifact-link" href="#"><span>📄</span> Deep dive → <code>docs/business_case.md</code></a>

</section>

<section class="proj-section" markdown="1">

## Proposed solution

To facilitate the process that goes from question to data product, I'm building the four AI workflows below:

1. **Automated data discovery.** Starts from a question and ends with a set of datasets, including a feasibility report (availability, quality, legal access, privacy...) and how to join the datasets together.
2. **Automated pretotyping.** Takes those datasets and their constraints, invents synthetic data where it is needed and proposes data product ideas to test market demand.
3. **Automated prototyping.** Turns a selected pretotype with proven market demand into a small version that actually works on Databricks, GCP or another platform to prove that it can be built.
4. **Automated build.** Takes a prototype and turns it into a full data product, complete with owner, lineage tracking, access control and more.


</section>
