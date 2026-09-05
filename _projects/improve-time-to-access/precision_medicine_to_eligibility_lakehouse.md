---
title: "DMD Eligibility Lakehouse"
excerpt: "A governed data platform linking genetic mutations in DMD to the therapies and trials a patient is eligible for."
date: 2026-07-31
status: "Under development"
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

<div class="glance">
  <div>
    <div class="glance-label">Problem</div>
    <p>Matching a DMD patient's mutation to eligible therapies and trials means manually cross-referencing registries, the reading-frame rule, and free-text trial criteria.</p>
  </div>
  <div>
    <div class="glance-label">What I built</div>
    <p>Phase 1 complete — 6 sources explored, first pipeline in build.</p>
  </div>
  <div>
    <div class="glance-label">Result</div>
    <p>Not yet measured. Check back once Epic 1 ships.</p>
  </div>
</div>

<section class="proj-section" markdown="1">

## Problem

The four FDA-approved exon-skipping therapies for DMD cover only 27% of patients by mutation alone. The other 73% have real options, but identifying which one a specific patient qualifies for means manually cross-referencing mutation registries, the reading-frame rule, and free-text trial eligibility criteria across systems that don't talk to each other — a process that doesn't scale across a registry and doesn't update when a trial opens or closes.

<a class="artifact-link" href="{{ '/primer/dmd-biology-treatment-and-eligibility/' | relative_url }}"><span>📖</span> New to DMD? → What is Duchenne muscular dystrophy?</a>
<a class="artifact-link" href="#"><span>📄</span> Deep dive → <code>docs/business_case.md</code></a>

</section>

<section class="proj-section" markdown="1">

## Data and constraints

Public, real sources cover the population-level side fully: LOVD, ClinVar, and HGMD for the mutation catalogue; ClinicalTrials.gov, the EU Clinical Trials Register, and FDA/EMA approvals for the trial catalogue. The patient-level side is where it gets constrained.

<div class="hero-diagram-wrap">
  <svg class="hero-diagram" viewBox="0 0 720 220" role="img" aria-label="Sources on the left -- LOVD, ClinVar, HGMD, ClinicalTrials.gov, EU CTR, FDA and EMA -- feeding a Bronze, Silver, Gold medallion pipeline in the middle, producing the patient trial eligibility data product on the right.">
    <defs>
      <marker id="arrow" viewBox="0 0 8 8" refX="7" refY="4" markerWidth="6" markerHeight="6" orient="auto-start-reverse">
        <path d="M0,0 L8,4 L0,8 Z"></path>
      </marker>
    </defs>
    <text class="stage-label" x="20" y="18">Sources</text>
    <text class="stage-label" x="300" y="18">Medallion pipeline</text>
    <text class="stage-label" x="600" y="18">Data product</text>

    <rect class="node" x="10" y="30" width="120" height="26" rx="6"></rect>
    <text x="20" y="47">LOVD · ClinVar · HGMD</text>
    <rect class="node" x="10" y="66" width="120" height="26" rx="6"></rect>
    <text x="20" y="83">ClinicalTrials.gov</text>
    <rect class="node" x="10" y="102" width="120" height="26" rx="6"></rect>
    <text x="20" y="119">EU CTR</text>
    <rect class="node" x="10" y="138" width="120" height="26" rx="6"></rect>
    <text x="20" y="155">FDA / EMA</text>

    <rect class="node" x="270" y="60" width="80" height="30" rx="6"></rect>
    <text x="285" y="80">Bronze</text>
    <rect class="node" x="370" y="60" width="80" height="30" rx="6"></rect>
    <text x="388" y="80">Silver</text>
    <rect class="node" x="470" y="60" width="80" height="30" rx="6"></rect>
    <text x="490" y="80">Gold</text>

    <rect class="node node--product" x="590" y="55" width="120" height="40" rx="8"></rect>
    <text class="product-text" x="602" y="80">patient_trial_​eligibility</text>

    <path d="M130,43 C 200,43 200,75 270,75" marker-end="url(#arrow)"></path>
    <path d="M130,79 C 200,79 200,75 270,75" marker-end="url(#arrow)"></path>
    <path d="M130,115 C 220,115 220,75 270,75" marker-end="url(#arrow)"></path>
    <path d="M130,151 C 220,151 220,75 270,75" marker-end="url(#arrow)"></path>
    <line x1="350" y1="75" x2="370" y2="75" marker-end="url(#arrow)"></line>
    <line x1="450" y1="75" x2="470" y2="75" marker-end="url(#arrow)"></line>
    <line x1="550" y1="75" x2="590" y2="75" marker-end="url(#arrow)"></line>
  </svg>
</div>

<div class="callout"><strong>Callout — what's redacted or missing:</strong> no real patient-level dataset exists for this project, and none is likely to — there's no data-sharing agreement with a registry like TREAT-NMD. Patient data is spoofed rather than real, a deliberate choice, not an oversight. This isn't a validated clinical system either: outputs are for hypothesis generation and pre-competitive research, not clinical decisions. Databricks Free Edition's data sharing is also more limited than a paid workspace, so even the synthetic demo will look somewhat artificial.</div>

<a class="artifact-link" href="#"><span>📄</span> Source-by-source notes → <code>exploratory/notes.md</code></a>

</section>

<section class="proj-section" markdown="1">

## Hypotheses and prototypes

Not "is this technically correct" — that gets checked against data and literature, quietly, in the background. This is "would anyone actually use it," and the only way to test that is to put a prototype in front of the person who'd use it and ask.

<div class="hyp-compact-list">
  <div class="hyp-row">
    <span class="verdict verdict--untested">Untested</span>
    <div class="hyp-row__text"><strong>A mutation-gap dashboard is valuable enough that a patient advocacy org would redirect funding based on it.</strong><span class="hyp-row__test">Test: walk a prototype past 2–3 contacts at groups like PPMD or TREAT-NMD; ask if it changes what they'd fund.</span><a class="hyp-row__proto-link" href="#"><span>🔗</span> Review the prototype (not built yet)</a></div>
  </div>
  <div class="hyp-row">
    <span class="verdict verdict--untested">Untested</span>
    <div class="hyp-row__text"><strong>A cohort-sizing tool is something a biotech program lead would actually use before writing a trial protocol.</strong><span class="hyp-row__test">Test: show it to someone who's scoped a protocol; ask whether it would've changed their early estimate.</span><a class="hyp-row__proto-link" href="#"><span>🔗</span> Review the prototype (not built yet)</a></div>
  </div>
  <div class="hyp-row">
    <span class="verdict verdict--untested">Untested</span>
    <div class="hyp-row__text"><strong>A point-of-care eligibility lookup fits into a real consultation, not just a demo.</strong><span class="hyp-row__test">Test: show a clinician or genetic counselor a mocked-up result; ask if they'd reach for it mid-visit.</span><a class="hyp-row__proto-link" href="#"><span>🔗</span> Review the prototype (not built yet)</a></div>
  </div>
</div>

<a class="artifact-link" href="#"><span>📄</span> Full hypothesis log, incl. the technical/scientific ones → <code>docs/hypotheses.md</code><span class="new-tag">Doesn't exist yet</span></a>

</section>

<section class="proj-section" markdown="1">

## Approach

A two-domain medallion architecture — Discovery (genetic and variant data) and Clinical (trial eligibility and matching) — with Discovery publishing `patient_mutation_profile` as a versioned cross-domain product that Clinical subscribes to.

<div class="rejected-box"><strong>Rejected:</strong> forcing genomic data into OHDSI's OMOP genomic extension, so the whole platform runs on one standard. Its own published coverage is still only ~50% VCF-to-OMOP for rare disease variants — a prototype, not a foundation. Kept HGVS / GA4GH VRS for variant representation instead, with OMOP reserved for the clinical domain where it's actually mature.</div>

<a class="artifact-link" href="#"><span>📄</span> Decision record → <code>docs/adr/adr_variant_representation_standard.md</code><span class="new-tag">Doesn't exist yet</span></a>

</section>

<section class="proj-section" markdown="1">

## What I built

<div class="built-box">
<p>Nothing shippable yet. Bronze source exploration across all six sources is complete; <code>clinical.gold.trial_eligibility_catalogue</code> is the first thing actually in build.</p>
<div class="built-links"><span>Repo: not yet public</span><span>Live demo: none yet</span></div>
</div>

<a class="artifact-link" href="#"><span>📄</span> Asset bundle → <code>trial_eligibility_catalogue/</code></a>

</section>

<section class="proj-section" markdown="1">

## Results

<div class="metric-row">
  <div class="metric-tile metric-tile--empty"><div class="metric-tile__value">—</div><div class="metric-tile__label">Time to match</div></div>
  <div class="metric-tile metric-tile--empty"><div class="metric-tile__value">—</div><div class="metric-tile__label">Mutation classes covered</div></div>
  <div class="metric-tile metric-tile--empty"><div class="metric-tile__value">—</div><div class="metric-tile__label">Real usage</div></div>
</div>

<a class="artifact-link" href="#"><span>📄</span> Schemas &amp; quality SLA → <code>docs/data-products.md</code></a>

</section>

<section class="proj-section" markdown="1">

## What I would change

*Too early to say — nothing's shipped yet to have regrets about. Revisit this after Epic 1.*

<a class="artifact-link" href="#"><span>📄</span> Version history → <code>docs/changelog.md</code></a>

</section>
