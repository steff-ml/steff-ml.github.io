# Outline: The dangers of depending on Claude, and the alternatives

> AI-written outline from the exponential-post interview. In progress, not final.
> Source post: `_posts/build-log/2026-10-24-model-sourcing.md` (unchanged).
> Next: `_ai-drafts/cost-per-task/outline.md`. Parked: `_ai-drafts/human-side/ideas.md`.

Series: Data + AI engineering on the exponential. Follows the clinical-trials post.
Qualitative, economic argument only. Written for both careers and businesses,
with a lesson for both.

## 1. The question
- Lens: what are the dangers for my career or business? (Human and societal
  lenses exist, but are out of scope.)
- What are the dangers of letting Claude do my work, and are there alternatives?

## 2. The approach
- Desk research: economic case studies of vendor-side dangers.
- Score the ways of getting AI in three layers:
  1. **Disqualifiers** (pass/fail): GDPR / data residency, EU AI Act, sector
     rules (e.g. DORA), export controls, usage policy.
  2. **Supply risk**: supplier power (price, rate limits, access), outages,
     deprecation, silent quality changes, lock-in.
  3. **Value**: capability, effort, cost.
- Citable basis: Kraljic matrix (supply risk x impact) for weighing; supplier
  power / hold-up (Porter, Williamson) for why the risks are structural.

## 3. What happened
1. Short description of the ways of procuring AI, with concrete vendor links.
   Long provider lists go to a concept page ("AI sourcing options").
2. The scored table (draft below).
3. Case studies (between table and portfolio): companies decide per task;
   high-volume or business-specific work moves to open/own models, the hardest
   work stays on the frontier. Supports "own the core, rent the edge".
4. My portfolio, and the incidents that made it necessary:
   - A genome-foundation-model question switched from Opus 5.5 to Opus 5
     (checked: on by default, with a notice, can be turned off).
   - The Fable export ban (12 June - 1 July 2026, checked): off for ALL customers.
     Afterwards Mythos 5 returned only for approved US organisations; Mythos
     Preview went first to Glasswing partners.
   - Price rise not yet; Uber shows the mechanism.
   - Effects, not motives: each restriction moves capability from me to someone
     else (the provider, its partners, other jurisdictions).
   - Disappointment: "The losses were minimal, but it did spook me." The case
     rests on what could happen. And paper scores can't replace evals.

### Table (draft scores approved by Steff, 2026-11-13)
DECIDED: pay-per-use split into closed vs open models; the biggest risk
difference is closed vs open weights, not rent vs buy.

| | Subscription | API, closed model | API, open model | Rented compute | Own hardware |
|---|:---:|:---:|:---:|:---:|:---:|
| ***Disqualifiers*** | | | | | |
| Data residency / GDPR | ✗ | ~ | ✓ | ✓ | ✓ |
| Export controls / access cut off | ✗ | ✗ | ✓ | ✓ | ✓ |
| Provider's usage policy | ✗ | ✗ | ~ | ✓ | ✓ |
| ***Supply risk*** | | | | | |
| Price and rate-limit power | ✗ | ✗ | ✓ | ✓ | ✓ |
| Silent quality changes | ✗ | ~ | ✓ | ✓ | ✓ |
| Outages / model retirement | ✗ | ✗ | ~ | ~ | ✓ |
| Lock-in | ✗ | ~ | ✓ | ✓ | ~ |
| ***Value*** | | | | | |
| Capability | ✓ | ✓ | ~ | ~ | ✗ |
| Effort | ✓ | ✓ | ✓ | ✗ | ✗ |
| Cost | ✓ | ~ | ✓ | ~ | ~ |

EU AI Act: in the text, not the table (deployer obligations barely depend on how
you get AI; fine-tuning can make you a provider).

## 4. The lesson
- Own the core, rent the edge.
- Treat knowledge (personal and business) as capital and make coherent
  investments in it. Use AI to automate what is not core to your offering.
- Two levels: a business that owns its processes; a person who owns their knowledge.
- Qualitative definition goes in this post:
  - Core = what sets you apart; you should own the knowledge of it.
  - Edge = anything that doesn't set you apart, or a new experiment that hasn't
    established itself yet.
  - Suggestion: tasks move: an experiment starts at the edge and becomes core as
    it proves itself and volume grows (link to the volume rule in post 2).
- Suggestion (open): make "take advantage of the situation" concrete: owning the
  core has never been this cheap (open weights near the frontier; you can own the
  model and still rent the compute).

## 5. What next
- The definition is clear in words, but where does a single task fall? The
  nature of the task decides where it should live -> segue to post 2.
- What is core, what is edge, and how do we decide that line quantitatively?
  -> cost-per-task post.
- Suggestion (open): "...and how much of it can numbers decide?" (core is a
  strategic choice; numbers decide where owning pays off for a given core).

## Open
- TODO(steff): revise portfolio rows.
- Verified 2026-11-13 (see draft footnotes): Fable ban, Glasswing, Windsurf and
  OpenAI cut-offs, Commercial Terms D.4, case studies, Kraljic, DORA, AI Act.
  Still (memory): Williamson, Porter.
- Case studies: Cognizant, Deloitte, Sierra (all-in); Uber (both); Revolut,
  Intercom, AT&T (open by default); Pinterest, C3 AI (own). Copilot dropped.
