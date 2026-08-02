---
title: "Product Definition: Linking DMD Mutations to Therapy and Trial Eligibility"
date: 2026-07-31 09:00:00 +0200
categories: [project-definition]
project: precision_medicine_to_eligibility_lakehouse
tags:
  - rare-disease
  - precision-medicine
  - data-mesh
  - agile
excerpt: "My current understanding of why and how to build the lakehouse for linking DMD mutations to trial eligibility."
---

*Part of [Rare disease variant to precision medicine eligibility]({{ "/projects/improve-time-to-access/precision_medicine_to_eligibility_lakehouse/" | relative_url }}).*

This is the data product definition document for the Rare disease variant to precision medicine eligibility project.
It is designed to help answer the value, feasbility and prioritization questions needed to start building
For more information on the what and the how of this document: [How and Why I Write Data Product Definition Documents]({{ "/primer/how-and-why-i-write-data-product-definition-documents/" | relative_url }}).
No code, just the argument for what to build first and why.

## Vision

A patient with a confirmed Duchenne muscular dystrophy (DMD) mutation has, on
paper, [several possible paths to treatment]({{ "/primer/dmd-biology-treatment-and-eligibility/#treatment-options" | relative_url }})[^1]
and should be able to immediately know for which they actually qualify for without needing a
week spent manually cross-referencing registries,
[the reading-frame rule]({{ "/primer/dmd-biology-treatment-and-eligibility/#reading-frame-rule" | relative_url }}),
and free-text trial criteria across systems not designed for this question.

A patient advocacy organization and pharmaceutical company should be able to immediately know 
which patient groups are underserved, so they can invest their euros where it matters and immediately find the patients
most likely to benefit from a particular treatment, so that potential life saving drugs do not fail
on trial enrolment problems.

An open, governed reference implementation of data products that link these pieces of information
should be extendable to other rare diseases in a low cost fashion.


This project is independent, OMOP CDM, open access, built for pre-competitive research and
hypothesis generation.[^4]

## Epics

Below is the vision translated into 5 epics.

### Epic 1 — Mutation Gap Analysis

**Story:** As a patient advocacy organisation, I want mutation classes with
no approved therapy and no active trial ranked by patient count, so I can
direct research funding at the largest unmet need.

**Value:** turns an approximate sense of "unmet need" into something specific
enough to act on.[^5]

**Feasibility:** An aggregation over the mutation and trial catalogues only:
no real patient data is available to me. The list of mutation classes with no approved therapy or active trial is valuable in its own right though
and I will spoof patient data to work through how it should be integrated.
Buildable as soon as both catalogues exist, and both are covered by real, public sources:
[LOVD](https://databases.lovd.nl/shared/genes/DMD), [ClinVar](https://www.ncbi.nlm.nih.gov/clinvar/),
and [HGMD](http://www.hgmd.cf.ac.uk/) for the mutation catalogue;
[ClinicalTrials.gov](https://clinicaltrials.gov), the [EU Clinical Trials Register](https://www.clinicaltrialsregister.eu),
and [FDA](https://www.fda.gov)/[EMA](https://www.ema.europa.eu) approvals for the trial catalogue.

**Per-epic success criteria:** a ranked, auto-refreshing view of uncovered
mutation classes. Patient counts are fictitious in this project.

### Epic 2 — Therapeutic Cohort Sizing

**Story:** As a biotech program lead, I want to size the addressable
population for a proposed mutation-eligibility rule before committing to a
trial protocol, so I can de-risk the investment.

**Value:** avoids designing a trial for a mutation class too small to
recruit.

**Feasibility:** the same population-level data as Epic 1, applied in the
reverse direction: the same real, public catalogues cover it. 

**Per-epic success criteria:** given candidate eligibility rules, a patient
count and characterization of the addressable population.

### Epic 3 — Patient-Trial Matching

**Story:** As a clinical research coordinator, I want to enter a patient's
confirmed mutation and get every open trial they meet the genetic criteria
for, so I can refer them without manually cross-referencing registries.

**Value:** the primary cross-domain product: replaces a manual,
expert-dependent process with an auditable query, at registry scale.[^5]

**Feasibility:** needs `patient_mutation_profile`, which depends on the
genomics layer (not yet built) and on patient data that doesn't exist for
this project, see Risks and assumptions. Higher feasibility risk than
Epics 1–2, and for reasons outside pure engineering.

**Per-epic success criteria:** given a confirmed HGVS variant, a ranked list
of eligible trials with evidence level and exclusion reasons, recomputed
automatically when a trial's criteria change.

### Epic 4 — Patient-Therapy Matching

**Story:** As a clinician, I want a patient's approved-therapy eligibility
with plain-language reasoning, so I can explain it to the patient and family
without re-deriving the reading-frame logic myself.

**Value:** replaces informal expert judgement with a reproducible, auditable
determination.[^6]

**Feasibility:** a constrained view of Epic 3 (evidence level = approved
only): same data-availability gap, smaller surface.

**Per-epic success criteria:** given a variant, approved therapies with
mutation-level reasoning in plain language.

### Epic 5 — Proactive Trial Alerts

**Story:** As a patient registry, I want to be notified automatically when a
registered patient becomes newly eligible for a trial, so recruitment
doesn't depend on someone re-running the match by hand.

**Value:** converts the system from a pull query into a push recruitment
tool, the highest-leverage epic for recruitment speed.

**Feasibility:** depends on a registry maintaining patient mutation profiles
inside its own system: a step further removed from reality than even
spoofed data can stand in for. See Risks and assumptions.

**Per-epic success criteria:** a delta between consecutive
eligibility-catalogue versions is surfaced as a notification when a trial's
status or criteria change.

## Prioritization

What's most important to do, and when: value, effort, and dependencies,
rated low/medium/high per epic rather than sorted into named buckets.

- **Epic 1 — Mutation Gap Analysis:** value high, effort low, dependencies
  none. Buildable immediately from public catalogues alone.
- **Epic 2 — Therapeutic Cohort Sizing:** value high, effort low,
  dependencies none. Same data as Epic 1, buildable alongside it.
- **Epic 3 — Patient-Trial Matching:** value highest, effort high,
  dependencies high — blocked on the genomics layer and on patient data that
  has to be spoofed rather than real.
- **Epic 4 — Patient-Therapy Matching:** value high, effort medium,
  dependencies the same as Epic 3, whose subset it is.
- **Epic 5 — Proactive Trial Alerts:** value high, effort medium,
  dependencies highest — the blocker isn't engineering, it's getting a
  registry to adopt the system into its own workflow.

Low effort and no dependencies put Epics 1 and 2 first, even though they
aren't the highest-value pair on their own: feasibility, not value alone,
sets the build order here.

Not doing, for now: a regulatory-submission platform (that's RDCA-DAP's job,
on CDISC SDTM, not this project's[^4]); a multi-disease implementation
before DMD ships, even though the architecture is built to generalise to
SMA, Huntington's, and the lysosomal storage disorders; and forcing genomic
data into the OMOP genomic extension, which is still ~50% VCF-coverage
prototype-grade.[^7]

Per the project's own README, Phase 1 (Bronze source exploration across
ClinicalTrials.gov, EU CTR/CTIS, FDA openFDA, LOVD, ClinVar, and Ensembl) is
complete, and `clinical.gold.trial_eligibility_catalogue` — the shared
dependency for Epics 1, 2, and 3 — is in build.

## Risks and assumptions

- What a data-sharing agreement with a registry like [TREAT-NMD](https://treat-nmd.eu)
  would actually require before real patient-level matching is possible is
  still unknown and may never resolve for an independent project. Until
  then, patient data is spoofed rather than real, a deliberate choice given
  that no institution grants that kind of access to an independent
  open-source project by default. Databricks Free
  Edition's data sharing is also more limited than a paid workspace, so even
  the synthetic cross-domain demo (Epics 3–5) will look somewhat artificial
  rather than production-grade.
- How much of Layer 3 (patient-level criteria: age, ambulatory status,
  antibody titres) belongs in this system, versus staying a field that
  requires clinical input at query time:  get this boundary wrong and the
  system either overreaches into clinical decision-making or undersells what
  it can answer alone.
- The reading-frame rule has known exceptions (roughly 8% of cases, mostly
  near the gene's 5′ end)[^6]: the system flags mutation eligibility; a
  clinician still owns the patient-level decision.
- HGVS / GA4GH VRS is assumed to be the right variant representation rather
  than the OMOP genomic extension: reasonable given the extension's ~50%
  VCF coverage today,[^7] but an assumption worth revisiting if that
  coverage improves.

## Why this is worth doing

The knowledge needed to answer "which trial is this patient eligible for"
already exists: in the reading-frame rule literature, in mutation
registries, in ClinicalTrials.gov. None of it is connected in a governed,
versioned way. That gap is exactly the kind of infrastructure problem this
site exists to work on. It is not about creating a new biological finding (others have way more expertise than I do),
but the translational engineering that turns existing and incoming treatments into something a
clinician, a trial sponsor, or a patient can actually query for a disease
where every month of delay is muscle that doesn't come back.

[^1]: Angulski et al. (2023) — DMD disease profile, mutation landscape, and current therapeutic approaches.
[^2]: Natural history of DMD: loss of ambulation by early adolescence, respiratory/cardiac failure typically fatal in the third decade.
[^3]: Thomas et al. (2022). [Average time to a confirmed DMD molecular diagnosis](https://pmc.ncbi.nlm.nih.gov/articles/PMC9308714/) — 2.2 years despite advances in NGS.
[^4]: Barrett et al. (2023). [RDCA-DAP](https://pmc.ncbi.nlm.nih.gov/articles/PMC10673974/) — the FDA/Critical Path Institute platform this project deliberately complements rather than duplicates.
[^5]: Leckie, Zia & Yokota (2024). [Applying every approved and experimental exon-skipping strategy to the full UMD-DMD mutation database](https://pmc.ncbi.nlm.nih.gov/articles/PMC11593839/) — the 27% coverage figure for approved therapies, and the quantification of what the remaining 73% need.
[^6]: Aartsma-Rus et al. (2009). [The reading-frame rule applied systematically to the Leiden DMD mutation database](https://pubmed.ncbi.nlm.nih.gov/19156838/) — the eligibility algorithm this project encodes as a computed field, including its known exceptions.
[^7]: Ahmadi et al. (2024). [OMOP CDM adapted for rare disease via a genomic extension](https://pmc.ncbi.nlm.nih.gov/articles/PMC11325822/) — reported ~50% VCF-to-OMOP coverage for rare disease variants.
