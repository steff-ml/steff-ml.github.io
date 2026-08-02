---
title: "Primer: Duchenne Muscular Dystrophy:  Biology, Treatment and Eligibility"
date: 2026-07-31 09:30:00 +0200
categories: [primer]
project: precision_medicine_to_eligibility_lakehouse
audience:
  - data-engineers
  - bio-scientists
tags:
  - rare-disease
  - molecular-biology
  - precision-medicine
excerpt: "Everything you need to know about Duchenne Muscular Dystrophy for this project"
---

If you want to build good data models, you must first understand the real-world
you are modelling. Therefore this primer will answer the following questions:
- What is Duchenne Muscular Dystrophy (DMD) and its cousin Becker Muscular Dystrophy?
- What role does the protein dystrophin play in this disease?
- Why do some mutations in the dystrophin gene cause DMD and others BMD?
- How can we treat DMD and when do the specific mutations matter?
- How does this translate into the definition of eligibility rules?


## What is DMD and BMD? {#what-is-dmd-and-bmd}

Duchenne muscular dystrophy (DMD) is a progressive, X-linked recessive muscle
disease affecting roughly **1 in 5,000 male births**, making it one of the
most common severe genetic disorders in humans.[^1][^2] Boys with DMD appear
normal at birth, but muscle deterioration starts very quickly after birth:

- **Ages 2–3:** waddling gait (duck-like walks), frequent falls, difficulty rising from the
  floor (Gowers' sign).
- **Ages 10–12:** most patients need a wheelchair.
- **Adolescence:** scoliosis (irregular back curve) and joint contractures develop, driving
  restrictive lung disease; assisted ventilation typically starts age 15–20.
- **Ages 20–30:** most patients die of respiratory or cardiac failure, even
  with optimal care.[^1]

Despite genetic testing being available, mean age at diagnosis is still
3.5–5 years — often a year or more after the first symptoms appear.[^2][^3]

**Becker muscular dystrophy (BMD)** is caused by mutations in the *same*
gene, but runs a much milder course:
patients often stay ambulatory (i.e. can still walk) into their 30s and have near-normal life spans. 
The difference in disease progression is the result of BMD mutation impairing the function of the dystrophin protein
less than DMD mutations.

## The role of dystrophin {#the-role-of-dystrophin}

DMD and BMD are both caused by mutations in the gene for **dystrophin**, a
protein that mechanically links the cell's internal cytoskeleton to the
extracellular matrix outside the cell. The cytoskeleton is a network of proteins (including actin)
that has many functions including providing each cell with their shape and mechanical
strength. The extracellular matrix is another network of proteins that embeds the specific cells
inside a tissue

![Diagram of dystrophin connecting intracellular actin to the extracellular matrix, via the dystroglycan complex](https://upload.wikimedia.org/wikipedia/commons/5/5e/Dystrophin_diagram.jpg)
*Dystrophin diagram by Daniel E. Michele and Kevin P. Campbell, via [Wikimedia Commons](https://commons.wikimedia.org/wiki/File:Dystrophin_diagram.jpg), [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).*

Structurally, dystrophin has four domains: an N-terminal domain that anchors
to internal actin filaments, a long central rod domain that acts as a shock
absorber, a cysteine-rich domain anchoring it to the cell membrane via a
protein complex, and a C-terminal domain that helps organise the membrane
around it.[^1] Functionally, that whole assembly exists to protect the
sarcolemma (the muscle cell's membrane) from tearing every time the muscle
contracts.

Take dystrophin away and the damage builds up over time: the
membrane becomes leaky and easily damaged → calcium floods in and disrupts
normal cell signalling → damaged cells leak proteins that trigger chronic
inflammation → the resulting mitochondrial stress and repeated
damage-and-repair cycle replaces muscle with fibrous scar tissue.[^1] 
- When this happens in skeletal muscle cells, the patient loses strength over time and becomes weelchair-bound,
- When this happens in heart muscle cells, the patient gets the dilated cardiomyopathy and eventual hearth failures that kill most of them.
- When this happens in the diaphragm muscles, the patient loses the ability to breath without assistance over time and can die of that too.

DMD and BMD are essentially both diseases where muscles progressively degrade over time, because dystrophin can't protect muscle cells from leaking.
The difference between the two is that DMD mutations completely destroy dystrophin functionality and BMD mutations impare, but don't fully destroy it.

## The genetics of dystrophin {#reading-frame-rule}

To see why *some* mutations in this one gene cause DMD and others cause the
much milder BMD, you need three facts about how a gene becomes a protein.

![Diagram of the central dogma: DNA is transcribed to RNA, then translated to protein](https://upload.wikimedia.org/wikipedia/commons/3/38/Central_Dogma_Model.png)
*Central dogma of molecular biology by Mike Jones, via [Wikimedia Commons](https://commons.wikimedia.org/wiki/File:Central_Dogma_Model.png), [CC BY-SA 2.5](https://creativecommons.org/licenses/by-sa/2.5/).*

1. A gene is DNA. Most of it isn't used directly. It's split into **exons**
   (the parts that get kept) and **introns** (the parts that get cut out).
   The cell splices the exons together into a shorter working copy called
   mRNA. The dystrophin gene is enormous (about 2.4 million DNA letters)
   across 79 exons, the largest gene in the human genome, which is also why
   it has an unusually high spontaneous mutation rate.[^1]
2. A ribosome reads that mRNA and builds a protein from it, one **codon** 
   (three letters) at a time. 
3. The ribosome starts reading at a fixed point (start codon) and reads unbroken groups of
   three, with no punctuation, all the way to a stop signal (stop codon). That's the
   **reading frame**.

Mutations that destroy the reading frame tend to cause the more severe DMD disease progression.
To understand why, picture the mRNA as a sentence made entirely of three-letter words, no spaces: 
`THE BIG RED FOX ATE THE CAT`.
Delete exactly one whole word and the rest still reads as real words: 
`THE BIG FOX ATE THE CAT`. 
Delete four letters instead of three,though, and every word after the cut is read starting from the wrong letter
`THE BIF OXA TET HEC AT`.
The ribosome doesn't know a mistake happened, so it just keeps reading
gibberish until it hits an accidental stop sign, almost immediately.

That's the reading frame rule: 
**if the number of DNA letters removed (or duplicated) is a multiple of three, everything downstream still reads correctly. If it isn't, everything downstream is scrambled.**[^4]

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
from a reference table.
The rule has known exceptions (roughly 8% of cases, mostly mutations near the
gene's start or in specific functional regions or stop codons that are introduced somewhere in the gene), 
which don't invalidate the rule so much as mark exactly where clinical judgement has to override a
computed answer.[^4]

So the difference in DMD and BMD is caused by the difference in producing a dystrophin protein that is completely versus partially non-functional.
In over 90% of known cases, this is usually the difference between a mutation that disrupts the reading frame and one that doesn't.
For this reason, restoring the reading frame is one of the many promising options to treat DMD.

## Treatment options for DMD {#treatment-options}

The treatment ideas for DMD can be organized into the following categories:
- Combat the consequences of the absence of dystrophin.
- Restore the reading frame to get BMD like functionality
- Provide other options than the broken dystrophin protein

Some of these treatment options only work on a subset of patients, while others could work on all.
There is no cure for DMD yet, only methods to slow down progression.

### Combat the consequences of the absence of dystrophin
While not addressing the cause of the disease itself, these drugs were the first to be able to stall progression of the disease and are in clinical use.
These drugs work independently of the exact mutation of the patient, but may induce long-term toxic effects, so some people might be excluded on medical grounds (age, disease stage, ...) from participating in clinical trials.

| Mechanism class | Intervenes at | Mutation-specific? | Key trade-off |
|---|---|---|---|
| **Anti-inflammatory** (glucocorticosteroids) | The inflammation cascade, not its cause | No — all patients eligible | Standard of care today, but only treats downstream symptoms; significant long-term side effects |
| **Membrane stabilisation** (e.g. poloxamer P188) | The leaky sarcolemma directly | No | Addresses the first domino, not the missing protein |



### Restore the reading frame to get BMD like functionality
The first instinct you may have to treat this disease is to fix the dystrophin gene itself using gene editing. This is often easier said than done, as there are limitations to our ability to deliver gene editing machinery inside a living being and reliably reach all muscle cells with that. Delivery of gene editing machinery is usually done using adenovirusses (AAV) and patients can develop an immune response to them, rendering the treatment moot.
There is also a limitation to how much an AAV virus can deliver and also a dilution effect if muscle cells die and are replaced by others that did not receive the fix. Most of these approaches are far from clinical practice.
For that reason, many drugs target the less ambitious goal of turning DMD into BMD. One promising approach is splice modulation: remove a broken exon from the mRNA to restore the reading frame. This requires lifelong dosing, but is achieved with small, easy to deliver molecules.
In any case, the applicability of these kinds of drugs are very dependent on the actual mutation a patient has and form the core reason for this project. 

| Mechanism class | Intervenes at | Mutation-specific? | Key trade-off |
|---|---|---|---|
| **Gene correction** (CRISPR-based: exon deletion, reframing, base editing, prime editing, HITI) | The reading frame itself[^4], via a permanent edit to the DNA rather than the spliced mRNA | **Yes**: different strategies rescue different mutation types | Could be one-time fixes instead of lifelong dosing, but nearly all strategies are still animal-model stage, not in patients |
| **Splice modulation** (AON exon skipping) | The reading frame itself, via steric-block splicing that masks a target exon from the spliceosome[^5] | **Yes**  each drug rescues one specific deletion pattern | Approved and in patients today, but the mutation itself is untouched, so dosing repeats indefinitely |
| **Stop-codon read-through** | Nonsense mutations specifically: forces the ribosome past a premature stop signal | **Yes** — nonsense mutations only (~10–15% of patients) | The one drug that reached late-stage trials (ataluren) failed its primary endpoints |

### Provide other options than the broken dystrophin protein
Some strategies opt to provide an alternative for the broken dystrophin protein. This includes providing a shortened semi-functional dystrophin variant developed in a lab, stimulating the expression of genes or injecting cells with a working dystrophin. Can work in all patients in principle, but still far from clinical practice. Providing external options carry immune response risks, either because of the delivery method or the foreign entity itself, so I suspect eligibility criteria to emerge on those grounds.

| Mechanism class | Intervenes at | Mutation-specific? | Key trade-off |
|---|---|---|---|
| **Gene replacement** (microdystrophin, AAV-delivered) | Supplies a working (shortened) gene copy regardless of the original mutation | No | **Patients with pre-existing AAV antibodies are excluded outright**[^6],  a hard patient-level criterion, not a mutation one, and satellite cells aren't reached, so the effect dilutes as muscle regenerates |
| **Compensatory upregulation** (utrophin, alpha-7 integrin, myostatin inhibition) | A different, structurally similar protein that can partly substitute for dystrophin | No | Lower immune risk than gene therapy since the protein is already the patient's own; evidence is still mostly preclinical |
| **Cell transplantation** | Delivers whole cells carrying functional dystrophin | No, in principle | No approach yet reliably clears safety, systemic delivery, *and* engraftment together |

Different therapies impose different eligibility criteria, ranging from standard clinical ones (pre-existing condition, age, ...) to mutation-therapy matching criteria.

## Therapeutic eligibility criteria for DMD {#eligibility-criteria}

Based on this situation, here is my working mental model of a three layered gate
on how to determine automatically whether a patient is eligible for a treatment. 

**Layer 1 — mutation-intrinsic classification.** Every patient record gets their precise mutation verified: 
variant class (deletion, duplication, nonsense, small indel, splice
site), which exons are affected, the computed reading-frame effect[^4],
whether the mutation sits in one of the two deletion hotspots (exons 3–9 and
45–55[^1]), and stop-codon type for nonsense mutations. 

**Layer 2 — approach-specific eligibility rules.** Each therapy in the table
above applies its own gate to Layer 1: an AON exon-51-skip rule only fires
for out-of-frame deletions that skipping exon 51 would rescue[^4][^5]; a
microdystrophin gene therapy rule fires for everyone, mutation-wise, because
it's mutation-agnostic.

**Layer 3 — patient-level criteria.** Applied only after Layer 1/2 mutation
eligibility is confirmed, and specific to each trial or therapy: age and
ambulatory status, cardiac and respiratory function thresholds, prior
treatment history, and **pre-existing AAV antibodies as a hard exclusion for every AAV-delivered gene therapy**,[^6] 
regardless of how well the patient's mutation matches the therapy.

## Conclusion
In this primer, we have discussed the disease progression of DMD versus BMD. 
We have seen that both are caused by a disfunctional dystrophin protein and that the difference between DMD and BMD is primarily explained by whether or not a mutation destroys the reading frame. 
We have seen that there is no universal fix for DMD and that it is therefore important to match patients to treatments that have the highest chance of working for them.
This primer will likely be updated once I go deeper into real clinical trial eligibility criteria, but it provides a mental model that will help you follow along the data modelling decisions I need to make throughout this project.


[^1]: Angulski et al. (2023). [Duchenne muscular dystrophy: disease mechanism and therapeutic strategies](https://www.frontiersin.org/journals/physiology/articles/10.3389/fphys.2023.1183101/full), *Frontiers in Physiology* — disease profile, dystrophin structure and function, mutation landscape, and therapeutic strategies.
[^2]: Crisafulli et al. (2020). [Global epidemiology of Duchenne muscular dystrophy: an updated systematic review and meta-analysis](https://pmc.ncbi.nlm.nih.gov/articles/PMC7275323/), *Orphanet Journal of Rare Diseases* — pooled global birth-prevalence estimate.
[^3]: Thomas et al. (2022). [Average time to a confirmed DMD molecular diagnosis](https://pmc.ncbi.nlm.nih.gov/articles/PMC9308714/) — 2.2 years despite advances in NGS.
[^4]: Aartsma-Rus et al. (2009). [The reading-frame rule applied systematically to the Leiden DMD mutation database](https://pubmed.ncbi.nlm.nih.gov/19156838/) — the eligibility algorithm this project encodes as a computed field, including its known exceptions.
[^5]: Leckie, Zia & Yokota (2024). [Applying every approved and experimental exon-skipping strategy to the full UMD-DMD mutation database](https://pmc.ncbi.nlm.nih.gov/articles/PMC11593839/) — coverage figures for approved and experimental exon-skipping approaches.
[^6]: [Binding and neutralizing anti-AAV antibodies: detection and implications for rAAV-mediated gene therapy](https://www.sciencedirect.com/science/article/pii/S1525001623000102), *Molecular Therapy* (2023) — pre-existing anti-AAV antibodies above a threshold are an accepted exclusion criterion across most rAAV gene therapy trials.
