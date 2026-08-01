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
excerpt: "Value, feasibility, and prioritization for linking DMD mutations to therapy and trial eligibility — structured as epics and scored against Agile frameworks, not argued for in one long narrative."
---

*Part of [Genetic disease variant to precision medicine eligibility]({{ "/projects/improve-time-to-access/precision_medicine_to_eligibility_lakehouse/" | relative_url }}).*

This is the product-definition post for this project — same job any
project-definition post does (is this worth building, what does done mean),
but structured as epics scored against value, feasibility, and business
viability rather than argued for as one narrative, per
[How I Write Product Requirement Documents]({{ "/primer/how-i-write-product-requirement-documents/" | relative_url }}).
No code — just the argument for what to build first and why.

## Vision

A patient with a confirmed Duchenne muscular dystrophy (DMD) mutation has, on
paper, [several possible paths to treatment]({{ "/primer/dmd-biology-treatment-and-eligibility/#treatment-options" | relative_url }})[^1]
— but should get every one they actually qualify for back as a query, not a
week spent manually cross-referencing registries,
[the reading-frame rule]({{ "/primer/dmd-biology-treatment-and-eligibility/#reading-frame-rule" | relative_url }}),
and free-text trial criteria across systems that don't talk to each other.
DMD is progressive — most patients lose ambulation by early adolescence[^2] —
and the average 2.2-year gap to a confirmed diagnosis[^3] is time already
lost before any of this even starts.

## Epics

Five epics, one per planned data product. Each carries its own value case and
feasibility read, per Cagan's four product risks: value, usability,
feasibility, and business viability[^4] — an idea that's valuable but
infeasible, or feasible but low-value, isn't ready to build regardless of how
the other looks. Each story below is also written to be independent and
testable in Bill Wake's INVEST sense[^5] — small enough to estimate on its
own, not a bundle of everything the epic could eventually grow into.

### Epic 1 — Mutation Gap Analysis

**Story:** As a patient advocacy organisation, I want mutation classes with
no approved therapy and no active trial ranked by patient count, so I can
direct research funding at the largest unmet need.

**Value:** turns an approximate sense of "unmet need" into something specific
enough to act on.[^6]

**Feasibility:** an aggregation over the mutation and trial catalogues only —
no patient data, no governance dependency. Buildable as soon as both
catalogues exist, and both are covered by real, public sources:
[LOVD](https://databases.lovd.nl/shared/genes/DMD), [ClinVar](https://www.ncbi.nlm.nih.gov/clinvar/),
and [HGMD](http://www.hgmd.cf.ac.uk/) for the mutation catalogue;
[ClinicalTrials.gov](https://clinicaltrials.gov), the [EU Clinical Trials Register](https://www.clinicaltrialsregister.eu),
and [FDA](https://www.fda.gov)/[EMA](https://www.ema.europa.eu) approvals for the trial catalogue.

**Definition of done:** a ranked, auto-refreshing view of uncovered mutation
classes by patient count.

### Epic 2 — Therapeutic Cohort Sizing

**Story:** As a biotech program lead, I want to size the addressable
population for a proposed mutation-eligibility rule before committing to a
trial protocol, so I can de-risk the investment.

**Value:** avoids designing a trial for a mutation class too small to
recruit.

**Feasibility:** the same population-level data as Epic 1, applied in the
reverse direction — the same real, public catalogues cover it, and no patient
data is required either.

**Definition of done:** given candidate eligibility rules, a patient count
and characterization of the addressable population.

### Epic 3 — Patient-Trial Matching

**Story:** As a clinical research coordinator, I want to enter a patient's
confirmed mutation and get every open trial they meet the genetic criteria
for, so I can refer them without manually cross-referencing registries.

**Value:** the primary cross-domain product — replaces a manual,
expert-dependent process with an auditable query, at registry scale.[^6]

**Feasibility:** needs `patient_mutation_profile`, which depends on the
genomics layer (not yet built). No real patient dataset is available or
likely to be — there's no data-sharing agreement with a registry — so this
runs on spoofed patient data instead. Higher feasibility risk than Epics 1–2,
and for reasons outside pure engineering.

**Definition of done:** given a confirmed HGVS variant, a ranked list of
eligible trials with evidence level and exclusion reasons, recomputed
automatically when a trial's criteria change.

### Epic 4 — Patient-Therapy Matching

**Story:** As a clinician, I want a patient's approved-therapy eligibility
with plain-language reasoning, so I can explain it to the patient and family
without re-deriving the reading-frame logic myself.

**Value:** replaces informal expert judgement with a reproducible, auditable
determination.[^7]

**Feasibility:** a constrained view of Epic 3 (evidence level = approved
only) — same dependency, same spoofed-patient-data caveat, smaller surface.

**Definition of done:** given a variant, approved therapies with
mutation-level reasoning in plain language.

### Epic 5 — Proactive Trial Alerts

**Story:** As a patient registry, I want to be notified automatically when a
registered patient becomes newly eligible for a trial, so recruitment
doesn't depend on someone re-running the match by hand.

**Value:** converts the system from a pull query into a push recruitment
tool — the highest-leverage epic for recruitment speed.

**Feasibility:** requires a registry to maintain patient mutation profiles
inside the system — a governance decision this project doesn't control, and
one step further removed from reality than spoofed data can stand in for.
The highest business-viability risk of the five, not a technical one.

**Definition of done:** a delta between consecutive eligibility-catalogue
versions is surfaced as a notification when a trial's status or criteria
change.

## Prioritization

MoSCoW[^8] over RICE[^9] here, deliberately: RICE's Reach and Confidence
numbers would be guesses before this has any real usage, and fake precision
is worse than an honest Must/Should/Could/Won't call.

- **Must have (v1):** Epic 1 (Mutation Gap Analysis) and Epic 2 (Cohort
  Sizing). Not because they're the highest-value epics — Epics 3 and 4
  clearly are — but because they're the only ones buildable without patient
  data or an external data-sharing agreement. Feasibility, not value, sets
  the build order here.
- **Should have (v1.1):** Epic 3 (Patient-Trial Matching) and Epic 4
  (Patient-Therapy Matching), once the genomics layer and a real patient data
  source exist.
- **Could have (later):** Epic 5 (Proactive Trial Alerts) — depends on
  registries adopting the system into their own workflow, not just this
  project shipping code.
- **Won't have (for now):** a regulatory-submission platform — that's
  RDCA-DAP's job, on CDISC SDTM, not this project's[^10]; a multi-disease
  implementation before DMD ships, even though the architecture is built to
  generalise to SMA, Huntington's, and the lysosomal storage disorders; and
  forcing genomic data into the OMOP genomic extension, which is still
  ~50% VCF-coverage prototype-grade.[^11]

Per the project's own README, Phase 1 (Bronze source exploration across
ClinicalTrials.gov, EU CTR/CTIS, FDA openFDA, LOVD, ClinVar, and Ensembl) is
complete, and `clinical.gold.trial_eligibility_catalogue` — the shared
dependency for Epics 1, 2, and 3 — is in build.

## Business viability

Distribution is open — Databricks Marketplace / API — rather than a closed
internal tool, so smaller biotechs and academic groups without bespoke
analytics teams can use it too. That positions this deliberately alongside,
not against, RDCA-DAP: RDCA-DAP is FDA-initiated, CDISC SDTM, restricted to
participating sponsors, built for regulatory submission; this project is
independent, OMOP CDM, open access, built for pre-competitive research and
hypothesis generation.[^10]

The real viability risk sits under Epics 3–5: what does a data-sharing
agreement with a registry like [TREAT-NMD](https://treat-nmd.eu) actually require before real
patient-level matching is possible? Until that exists — and it may never,
for this project — patient data is spoofed rather than real, which is a
deliberate choice, not an oversight: it's the only way to demonstrate the
patient-facing epics at all without institutional access nobody grants an
independent open-source project by default. Databricks Free Edition's data
sharing is also limited compared to a paid workspace, so even the synthetic
cross-domain demo will look somewhat artificial rather than production-grade.
That answer determines how much of the patient-facing value can be
demonstrated publicly versus only described — and it's a governance and
tooling question, not an engineering one.

## Risks and assumptions

- The HITI (knock-in) eligibility caveat is unresolved: if an exon is knocked
  in at a junction site, does that change what "in the exon region" means for
  frame-effect purposes? Flagged in the scientific background doc, not
  answered.
- How much of Layer 3 (patient-level criteria: age, ambulatory status,
  antibody titres) belongs in this system, versus staying a field that
  requires clinical input at query time — get this boundary wrong and the
  system either overreaches into clinical decision-making or undersells what
  it can answer alone.
- The reading-frame rule has known exceptions (roughly 8% of cases, mostly
  near the gene's 5′ end)[^7] — the system flags mutation eligibility; a
  clinician still owns the patient-level decision.
- HGVS / GA4GH VRS is assumed to be the right variant representation rather
  than the OMOP genomic extension — reasonable given the extension's ~50%
  VCF coverage today,[^11] but an assumption worth revisiting if that
  coverage improves.

## Why this is worth doing

The knowledge needed to answer "which trial is this patient eligible for"
already exists — in the reading-frame rule literature, in mutation
registries, in ClinicalTrials.gov. None of it is connected in a governed,
versioned way. That gap is exactly the kind of infrastructure problem this
site exists to work on: not a new biological finding, but the translational
engineering that turns an existing scientific finding into something a
clinician, a trial sponsor, or a patient can actually query — for a disease
where every month of delay is muscle that doesn't come back.

[^1]: Angulski et al. (2023) — DMD disease profile, mutation landscape, and current therapeutic approaches.
[^2]: Natural history of DMD: loss of ambulation by early adolescence, respiratory/cardiac failure typically fatal in the third decade.
[^3]: Thomas et al. (2022). [Average time to a confirmed DMD molecular diagnosis](https://pmc.ncbi.nlm.nih.gov/articles/PMC9308714/) — 2.2 years despite advances in NGS.
[^4]: Marty Cagan, [The Four Big Risks](https://www.svpg.com/four-big-risks/), Silicon Valley Product Group (from *INSPIRED*) — value, usability, feasibility, business viability.
[^5]: Bill Wake (2003). [INVEST in Good Stories, and SMART Tasks](https://xp123.com/invest-in-good-stories-and-smart-tasks/).
[^6]: Leckie, Zia & Yokota (2024). [Applying every approved and experimental exon-skipping strategy to the full UMD-DMD mutation database](https://pmc.ncbi.nlm.nih.gov/articles/PMC11593839/) — the 27% coverage figure for approved therapies, and the quantification of what the remaining 73% need.
[^7]: Aartsma-Rus et al. (2009). [The reading-frame rule applied systematically to the Leiden DMD mutation database](https://pubmed.ncbi.nlm.nih.gov/19156838/) — the eligibility algorithm this project encodes as a computed field, including its known exceptions.
[^8]: [What is MoSCoW Prioritization?](https://www.agilebusiness.org/resource/what-is-moscow-prioritization/), Agile Business Consortium (DSDM).
[^9]: [RICE: Simple Prioritization for Product Managers](https://www.intercom.com/blog/rice-simple-prioritization-for-product-managers/), Intercom.
[^10]: Barrett et al. (2023). [RDCA-DAP](https://pmc.ncbi.nlm.nih.gov/articles/PMC10673974/) — the FDA/Critical Path Institute platform this project deliberately complements rather than duplicates.
[^11]: Ahmadi et al. (2024). [OMOP CDM adapted for rare disease via a genomic extension](https://pmc.ncbi.nlm.nih.gov/articles/PMC11325822/) — reported ~50% VCF-to-OMOP coverage for rare disease variants.
