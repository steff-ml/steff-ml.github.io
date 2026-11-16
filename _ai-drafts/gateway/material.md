# Raw material: the gateway post (blog 3)

> AI-written (exponential-post skill, move 1, step 1: gather). Material, not
> a skeleton. Source: the private `ai-stack` repo (branch
> `feature/ai-stack-improvement`, PR #1, not merged), its decision journal
> `docs/journal/2026-11-stack-review.md` (written as "raw material for a
> write-up"), ADR-001/002/008, and the Claude Code session of 2026-11-11.
> Security details are kept general on purpose: this repo is public.

## Where it sits in the series

Blog 2 ends: build the gateway infrastructure and measure what it really
costs. Blog 2's claims that this post can test:
- the fallback is a switching function; switching should be cheap;
- make = own the process, buy the model as a commodity;
- small models are bad at undecomposed tasks, not at tasks;
- AI lowers the cost of building, so the make threshold drops;
- the floor must clear the bar per task (untested).

## What exists

| Layer | What | State |
|---|---|---|
| Infra | LiteLLM gateway on the PC; task routes `routine`, `commit`, `review`, `embed`; one declared fallback per chat route at the other EU provider; budgets in four layers; a key per tool; secret guard; response cache; traces to Langfuse | Built; tested against local mocks, not yet live on the real machine |
| Harness | OpenCode wired to the gateway; house rules; Databricks skills; `/commit`, `/review` | Built |
| Evals | 8 cases per chat route (incl. Databricks/PySpark routine tasks), deterministic checks, pytest, Claude as judge, human calibration | Built, never run on real data |
| Loop | Monthly trace digest → review → one commit per finding; monthly catalog watch | Built; no traces yet |

"Not finished" (Steff): PR #1 unmerged; live calls, fallback ids, Langfuse
tags, OpenCode permissions and provider spending caps still unverified on
the real machine.

## Cost data available now

### Building (the investment)
| When | What | Evidence | Model cost | Human time |
|---|---|---|---|---|
| 2026-08-29/30 | First build: gateway, monitoring, spend review, harness | 8 commits in two days (incl. ~20k lines of mostly vendored skills) | TODO: VS Code sessions, not yet mined | TODO(steff) |
| 2026-11-11 | Full review + hardening: 23 findings, ~20 commits, ~7,800 lines | One Claude Code session | **$29.11** at list price (Opus), paid by subscription | TODO(steff) |

### Maintaining (model churn)
| When | Change | Cost noted in the ADRs |
|---|---|---|
| 2026-09-04 | Two models out, `deepseek-v4-flash` the default coder, `complex` route retired | "better and barely dearer" |
| 2026-09-05 | `commit`: gpt-oss-20b → Qwen3.5-9B | input 2.5x dearer on the highest-frequency route, accepted: "a commit message you rewrite by hand costs more than the tokens" |
| 2026-10-24 | `commit` routed to Qwen3.5-9B (OVH) | one-line edit |
| 2026-11-11 | Drift review | "most findings were drift, not bugs": ADRs promised what the code never did |

Each swap was "a one-line edit plus a gateway restart". The honest cost,
written down in ADR-001 in August: the plumbing is reversible, prompts tuned
to one model don't transfer. Every swap needs an eval pass.

### Running (prices in the config, EUR per 1M tokens in / out)
| Route | Model @ provider | Price |
|---|---|---|
| routine | deepseek-v4-flash @ Scaleway | 0.40 / 0.80 (0.08 cached in) |
| commit | Qwen3.5-9B @ OVH | 0.10 / 0.15 |
| review | Qwen3.8-27B @ OVH | 0.40 / 2.70 |
| embed | Qwen3-Embedding-8B @ OVH | 0.10 / free |

Budgets: OpenCode key EUR 15/month, scripts EUR 5/month.
Actual spend: not in the repo. TODO(steff): LiteLLM admin UI / Langfuse.
TODO: frontier list prices for the same work, checked, for comparison.

## Material that supports blog 2 (from the repo, not invented)

- **Small models fail on undecomposed, stateful work** (ADR-007): a `commit`
  subagent on a small model wrote a message and stopped without committing;
  a `review` subagent guessed the wrong diff. Fix: commands that inject the
  state and end in a checkable state. The task got decomposed, not the
  model upgraded.
- **Human time beats token price** (ADR-002, 2026-09-05): a 2.5x dearer input
  price was accepted because rewriting a commit message by hand costs more.
- **Make the process, buy the commodity** was already in ADR-002 in August:
  "buy inference (renting beats owning at this volume), build the gateway /
  skill / eval layer."
- **Switching is cheap, but not free**: one-line swaps vs prompts that don't
  transfer.

## Disappointments and surprises (journal part 3)

1. A rejected request was still logged, secret included, to the trace
   backend; a naive block would have leaked what it blocked.
2. The tracing plugin was a bigger leak than the gateway.
3. Pinning forced an upgrade that had been deferred.
4. An assumption about the budget database was wrong (the code said the
   opposite).
5. Fallbacks silently didn't work with per-tool keys; found only with a
   deliberately failing primary.
6. Most findings were drift: written promises the code never kept.

Theme: the cost of owning is not the code (AI wrote it), it is **noticing**:
verification, drift, assumptions. Matches blog 2's "what remains of the
investment: specifying, decomposing, verifying, noticing change".

## Decided and not done
Mobile access; running the monthly review in the cloud; self-hosting
Langfuse now.

## Candidate questions (for the dialogue, not decided)

1. What does it really cost to build and run the fallback? (blog 2's What next)
2. Does the floor clear the bar on my tasks? (needs an eval run)
3. Where does the cost of owning AI infrastructure actually go, once AI
   writes the code?

## Gaps

- Steff's own time, per build phase.
- Actual spend per route (LiteLLM UI / Langfuse), ideally a month.
- Cost of the August build sessions (VS Code bridge sessions).
- An eval run on the real machine: floor vs a frontier reference.
- Frontier prices for comparison (checked).
