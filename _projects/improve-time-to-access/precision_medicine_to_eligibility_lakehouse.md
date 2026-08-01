---
title: "Rare disease variant to precision medicine eligibility"
excerpt: "A governed data platform linking genetic mutations in DMD to the therapies and trials a patient is eligible for."
date: 2026-07-31
status: "Under development"
mission: improve-time-to-access
header:
  teaser: /assets/images/projects/precision-medicine-eligibility-teaser.jpg
# tags let you group projects later:
tags:
  - databricks
  - rare-disease
  - precision-medicine
  - lakehouse
---

![Diagram of dystrophin connecting intracellular actin to the extracellular matrix](/assets/images/projects/precision-medicine-eligibility-teaser.jpg){: width="450"}
*Dystrophin diagram by Daniel E. Michele and Kevin P. Campbell, via [Wikimedia Commons](https://commons.wikimedia.org/wiki/File:Dystrophin_diagram.jpg), [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).*

*Status: under development.*

Rare diseases form a heterogeneous group of diseases that together affect about 5%
of the world population. Many of these diseases are caused by discrete mutations in 
and around a single gene that cause its product to be non-functional,
such as Duchenne Muscular Dystrophy (DMD), spinal muscular atrophy,
Huntington's disease and the lysosomal storage disorders. 
However, many distinct mutations can cause a gene to lose its function, hence 
many different genetic variants have been identified for these diseases.
Precision medicines that target the specific mutations have been promising for the treatment of
these rare diseases, but they only work against a subset of patients. 
This creates a significant data challenge: the genetic variant of each patient should be precisely mapped
to the properties of the precision drug to ensure a perfect match or the drug won't work at all. 
Since this information resides in different data sources, the current process often involves having an
expert manually going through different databases to figure out these matches, creating devastating delays in treatments and problems in proving the efficacy of promising drugs.

**The goal:** build a reference implementation for a governed data platform
that turns a confirmed genetic variant into a full eligibility picture (approved therapies, open trials, and the gaps neither one covers)
, updated automatically as the trial landscape changes.

DMD is the starting implementation, but the underlying model generalises
directly to other rare genetic diseases where a discrete genetic measurement
determines therapeutic eligibility: spinal muscular atrophy, Huntington's
disease, the lysosomal storage disorders. It also increasingly generalizes
to diseases like cancer that were not considered rare before, but where deepening
research is uncovering genetic subtypes that matter for the efficacy of treatment.
Hence, I believe a similar data architecture can be reused for these different diseases.

<!-- TODO as this develops, I will add.
     - An architecture diagram (drop in assets/images/projects/ and reference it)
     - A "what's built so far" section with concrete progress
     - A link to the repo, if/when it's public
     - A worked example: one patient's HGVS variant walked through to a
       ranked list of eligible trials
-->
