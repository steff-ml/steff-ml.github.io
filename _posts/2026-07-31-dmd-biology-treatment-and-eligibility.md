---
title: "Primer: Duchenne Muscular Dystrophy — Biology, Treatment, and Eligibility"
date: 2026-07-31 09:30:00 +0200
categories: [primer]
project: precision_medicine_to_eligibility_lakehouse
audience:
  - data-engineers
  - bio-scientists
tags:
  - genetic-disease
  - molecular-biology
  - precision-medicine
excerpt: "The DMD biology this project's data model depends on, end to end: the disease, dystrophin's role, the genetics behind the reading-frame rule, the treatment landscape, and how eligibility criteria are actually structured."
---

Five questions, in the order you'd actually need to answer them to model this
disease's data: what is the disease, what protein is broken, why does a
genetic mutation determine severity, what can be done about it, and how do
you decide who qualifies for what. No biology background assumed.

## What is DMD and BMD? {#what-is-dmd-and-bmd}

Duchenne muscular dystrophy (DMD) is a progressive, X-linked recessive muscle
disease affecting roughly **1 in 5,000 male births**, making it one of the
most common severe genetic disorders in humans.[^1][^2] Boys with DMD appear
normal at birth; weakness starts in the hips and thighs before spreading. The
course is severe and predictable:

- **Ages 2–3:** waddling gait, frequent falls, difficulty rising from the
  floor (Gowers' sign).
- **Ages 10–12:** most patients need a wheelchair.
- **Adolescence:** scoliosis and joint contractures develop, driving
  restrictive lung disease; assisted ventilation typically starts age 15–20.
- **Ages 20–30:** most patients die of respiratory or cardiac failure, even
  with optimal care.[^1]

Despite genetic testing being available, mean age at diagnosis is still
3.5–5 years — often a year or more after the first symptoms appear.[^2][^3]

**Becker muscular dystrophy (BMD)** is caused by mutations in the *same*
gene and is, on paper, the same disease — but runs a much milder course:
patients often stay ambulatory into their 30s. The reason two diseases come
out of one gene is the entire subject of this primer, and it's the reason
this project's data model exists at all.

## The role of dystrophin {#the-role-of-dystrophin}

DMD and BMD are both caused by mutations in the gene for **dystrophin**, a
protein that mechanically links the cell's internal cytoskeleton to the
extracellular matrix outside the cell.

![Diagram of dystrophin connecting intracellular actin to the extracellular matrix, via the dystroglycan complex](https://upload.wikimedia.org/wikipedia/commons/5/5e/Dystrophin_diagram.jpg)
*Dystrophin diagram by Daniel E. Michele and Kevin P. Campbell, via [Wikimedia Commons](https://commons.wikimedia.org/wiki/File:Dystrophin_diagram.jpg), [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).*

Structurally, dystrophin has four domains: an N-terminal domain that anchors
to internal actin filaments, a long central rod domain that acts as a shock
absorber, a cysteine-rich domain anchoring it to the cell membrane via a
protein complex, and a C-terminal domain that helps organise the membrane
around it.[^1] Functionally, that whole assembly exists to protect the
sarcolemma (the muscle cell's membrane) from tearing every time the muscle
contracts.

Take dystrophin away and the damage cascades in a fairly direct line: the
membrane becomes leaky and easily damaged → calcium floods in and disrupts
normal cell signalling → damaged cells leak proteins that trigger chronic
inflammation → the resulting mitochondrial stress and repeated
damage-and-repair cycle replaces muscle with fibrous scar tissue.[^1] Run
that cascade in heart muscle and you get the dilated cardiomyopathy that
kills many patients; run it in the diaphragm and you get the ventilatory
failure that kills the rest.

## The genetics of dystrophin {#reading-frame-rule}

To see why *some* mutations in this one gene cause DMD and others cause the
much milder BMD, you need three facts about how a gene becomes a protein.

![Diagram of the central dogma: DNA is transcribed to RNA, then translated to protein](https://upload.wikimedia.org/wikipedia/commons/3/38/Central_Dogma_Model.png)
*Central dogma of molecular biology by Mike Jones, via [Wikimedia Commons](https://commons.wikimedia.org/wiki/File:Central_Dogma_Model.png), [CC BY-SA 2.5](https://creativecommons.org/licenses/by-sa/2.5/).*

1. A gene is DNA. Most of it isn't used directly — it's split into **exons**
   (the parts that get kept) and **introns** (the parts that get cut out).
   The cell splices the exons together into a shorter working copy called
   mRNA. The dystrophin gene is enormous — about 2.4 million DNA letters
   across 79 exons, the largest gene in the human genome, which is also why
   it has an unusually high spontaneous mutation rate.[^1]
2. A ribosome reads that mRNA and builds a protein from it, one **codon** —
   three letters — at a time.
3. The ribosome starts reading at a fixed point and reads unbroken groups of
   three, with no punctuation, all the way to a stop signal. That's the
   **reading frame**.

Here's the consequence that matters. Picture the mRNA as a sentence made
entirely of three-letter words, no spaces: `THE BIG RED FOX ATE THE CAT`.
Delete exactly one whole word — "RED" — and the rest still reads as real
words: `THE BIG FOX ATE THE CAT`. Delete four letters instead of three,
though, and every word after the cut is read starting from the wrong letter
— the ribosome doesn't know a mistake happened, so it just keeps reading
gibberish until it hits an accidental stop sign, almost immediately.

That's the reading frame rule: **if the number of DNA letters removed (or
duplicated) is a multiple of three, everything downstream still reads
correctly. If it isn't, everything downstream is scrambled.**[^4]

```
reading_frame_effect = (nucleotides deleted or duplicated) mod 3
```

- **Result = 0 → in-frame.** The ribosome still produces a shorter, partially
  functional dystrophin. This is **BMD**: missing a real chunk of the
  protein, but enough of it works to substantially slow the disease.
- **Result = 1 or 2 → out-of-frame.** The frame shifts, the ribosome hits a
  premature stop almost immediately, and no usable dystrophin is produced at
  all. This is **DMD**.

Because every exon has a fixed, published length, this is fully computable
from a reference table — it is never something a person annotates by hand.
The rule has known exceptions (roughly 8% of cases, mostly mutations near the
gene's start or in specific functional regions), which don't invalidate the
rule so much as mark exactly where clinical judgement has to override a
computed answer.[^4]

## Treatment options for DMD {#treatment-options}

The cleanest way to organise DMD therapies isn't by drug name — it's by
*which point in the dystrophin story each one intervenes at*, because that's
what determines who's eligible and what the trade-offs are.

| Mechanism class | Intervenes at | Mutation-specific? | Key trade-off |
|---|---|---|---|
| **Anti-inflammatory** (glucocorticosteroids) | The inflammation cascade, not its cause | No — all patients eligible | Standard of care today, but only treats downstream symptoms; significant long-term side effects |
| **Membrane stabilisation** (e.g. poloxamer P188) | The leaky sarcolemma directly | No | Addresses the first domino, not the missing protein |
| **Splice modulation** (AON exon skipping) | The reading frame itself, via steric-block splicing that masks a target exon from the spliceosome[^5] | **Yes** — each drug rescues one specific deletion pattern | Approved and in patients today, but the mutation itself is untouched, so dosing repeats indefinitely |
| **Gene correction** (CRISPR-based: exon deletion, reframing, base editing, prime editing, HITI) | The reading frame itself[^4], via a permanent edit to the DNA rather than the spliced mRNA | **Yes** — different strategies rescue different mutation types | Could be one-time fixes instead of lifelong dosing, but nearly all strategies are still animal-model stage, not in patients |
| **Gene replacement** (microdystrophin, AAV-delivered) | Supplies a working (shortened) gene copy regardless of the original mutation | No | **Patients with pre-existing AAV antibodies are excluded outright**[^6] — a hard patient-level criterion, not a mutation one — and satellite cells aren't reached, so the effect dilutes as muscle regenerates |
| **Compensatory upregulation** (utrophin, alpha-7 integrin, myostatin inhibition) | A different, structurally similar protein that can partly substitute for dystrophin | No | Lower immune risk than gene therapy since the protein is already the patient's own; evidence is still mostly preclinical |
| **Stop-codon read-through** | Nonsense mutations specifically — forces the ribosome past a premature stop signal | **Yes** — nonsense mutations only (~10–15% of patients) | The one drug that reached late-stage trials (ataluren) failed its primary endpoints |
| **Cell transplantation** | Delivers whole cells carrying functional dystrophin | No, in principle | No approach yet reliably clears safety, systemic delivery, *and* engraftment together |


## Therapeutic eligibility criteria for DMD {#eligibility-criteria}

Eligibility for a given therapy is a three-layer question. Worth being
upfront that this specific three-layer split — mutation-intrinsic,
approach-specific, patient-level — isn't a named framework lifted from a
paper; it's this project's own data-modelling structure for organising
eligibility logic. What's independently verifiable is the content of each
layer, not the three-layer packaging itself. The layers still have to stay
separate for a concrete, checkable reason: you can be mutation-eligible for a
therapy and still excluded on clinical grounds, or the reverse.

**Layer 1 — mutation-intrinsic classification.** Every patient record starts
here: variant class (deletion, duplication, nonsense, small indel, splice
site), which exons are affected, the computed reading-frame effect[^4],
whether the mutation sits in one of the two deletion hotspots (exons 3–9 and
45–55[^1]), and stop-codon type for nonsense mutations. All of this is
computed from the variant itself — no clinical input required.

**Layer 2 — approach-specific eligibility rules.** Each therapy in the table
above applies its own gate to Layer 1: an AON exon-51-skip rule only fires
for out-of-frame deletions that skipping exon 51 would rescue[^4][^5]; a
microdystrophin gene therapy rule fires for everyone, mutation-wise, because
it's mutation-agnostic.

**Layer 3 — patient-level criteria.** Applied only after Layer 1/2 mutation
eligibility is confirmed, and specific to each trial or therapy: age and
ambulatory status, cardiac and respiratory function thresholds, prior
treatment history, and — the concrete example that motivated separating
these layers in the first place — **pre-existing AAV antibodies as a hard
exclusion for every AAV-delivered gene therapy**,[^6] regardless of how well
the patient's mutation matches the therapy.

Getting a patient's full eligibility picture means evaluating all three
layers, in order, for every therapy and trial they might qualify for — which
is exactly the join this project's data products exist to compute
automatically instead of by hand.

[^1]: Angulski et al. (2023). [Duchenne muscular dystrophy: disease mechanism and therapeutic strategies](https://www.frontiersin.org/journals/physiology/articles/10.3389/fphys.2023.1183101/full), *Frontiers in Physiology* — disease profile, dystrophin structure and function, mutation landscape, and therapeutic strategies.
[^2]: Crisafulli et al. (2020). [Global epidemiology of Duchenne muscular dystrophy: an updated systematic review and meta-analysis](https://pmc.ncbi.nlm.nih.gov/articles/PMC7275323/), *Orphanet Journal of Rare Diseases* — pooled global birth-prevalence estimate.
[^3]: Thomas et al. (2022). [Average time to a confirmed DMD molecular diagnosis](https://pmc.ncbi.nlm.nih.gov/articles/PMC9308714/) — 2.2 years despite advances in NGS.
[^4]: Aartsma-Rus et al. (2009). [The reading-frame rule applied systematically to the Leiden DMD mutation database](https://pubmed.ncbi.nlm.nih.gov/19156838/) — the eligibility algorithm this project encodes as a computed field, including its known exceptions.
[^5]: Leckie, Zia & Yokota (2024). [Applying every approved and experimental exon-skipping strategy to the full UMD-DMD mutation database](https://pmc.ncbi.nlm.nih.gov/articles/PMC11593839/) — coverage figures for approved and experimental exon-skipping approaches.
[^6]: [Binding and neutralizing anti-AAV antibodies: detection and implications for rAAV-mediated gene therapy](https://www.sciencedirect.com/science/article/pii/S1525001623000102), *Molecular Therapy* (2023) — pre-existing anti-AAV antibodies above a threshold are an accepted exclusion criterion across most rAAV gene therapy trials.
