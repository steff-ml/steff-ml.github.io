---
title: "Genetic disease variant to precision medicine eligibility"
excerpt: "A governed data platform linking confirmed genetic mutations in rare diseases like Duchenne muscular dystrophy to the therapies and trials a patient is eligible for."
date: 2026-07-31
status: "Under development"
mission: improve-time-to-access
header:
  teaser: /assets/images/projects/precision-medicine-eligibility-teaser.svg   # TODO: add a teaser image (optional)
# tags let you group projects later:
tags:
  - databricks
  - genetic-disease
  - precision-medicine
  - lakehouse
---

*Status: under development.*

A patient with a confirmed Duchenne muscular dystrophy (DMD) mutation should
be able to find every therapy and trial they qualify for as a query — not a
week spent manually cross-referencing registries, the reading-frame rule, and
free-text trial criteria across systems that don't talk to each other.

**The goal:** build a reference implementation for a governed data platform
that turns a confirmed genetic variant into a full eligibility picture —
approved therapies, open trials, and the gaps neither one covers — updated
automatically as the trial landscape changes.

DMD is the starting implementation, but the underlying model generalises
directly to other rare genetic diseases where a discrete genetic measurement
determines therapeutic eligibility: spinal muscular atrophy, Huntington's
disease, the lysosomal storage disorders.

<!-- TODO as this develops, I will add.
     - An architecture diagram (drop in assets/images/projects/ and reference it)
     - A "what's built so far" section with concrete progress
     - A link to the repo, if/when it's public
     - A worked example: one patient's HGVS variant walked through to a
       ranked list of eligible trials
-->
