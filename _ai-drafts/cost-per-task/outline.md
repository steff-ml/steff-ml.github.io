# Outline post 2: What does an AI task really cost?

> AI-written outline from the exponential-post interview. In progress, not final.
> Follows post 1 (`_ai-drafts/model-sourcing/outline.md`).
> Useful data point from post 1 research: Vercel AI Gateway, Aug 2026: open-weight
> models 56% of tokens but 14% of spend (volume to open, money to frontier).

Character: quantitative. How to calculate it, and under which conditions the
company strategies from post 1 are a good choice. Plus the worked example.

## Decisions (round 3, 2026-11-13) -- supersedes the round-2 question
- Post 1 now ends on "own the core, rent the edge", so this post's question is:
  what is core, what is edge, and how do we decide that line quantitatively?
  Suggestion: "...and how much of it can numbers decide?"
- Steff's starting guideline: volume. Tasks run at high volume are both core to
  the strategy AND cheaper to own. Not a perfect line, but a good start.
- Literature (memory, to check):
  - Support: Williamson, transaction cost economics (frequency, together with
    asset specificity, favours making over buying); Kraljic (profit impact is
    partly volume purchased); cloud repatriation (a16z "The Cost of Cloud, a
    Trillion Dollar Paradox", 2021; 37signals leaving the cloud): rent spiky or
    exploratory workloads, own the steady baseline. Cases: Intercom, Revolut, Uber.
  - Pushback: Prahalad & Hamel (1990): core = customer value, hard to imitate,
    not volume. Payroll = high volume, not core. Fine & Whitney (1996):
    dependence for capacity (fine to outsource) vs for knowledge (core).
  - Suggestion: 2x2 of volume (numbers decide) x differentiation (strategic
    choice). Volume rule covers the high-volume differentiating corner.

- Moved from post 1 (checked 2026-11-13): Vercel AI Gateway, Aug 2026: open-weight
  56% of tokens (7% in Dec 2025) but 14% of spend
  (https://vercel.com/blog/ai-gateway-production-index-september-2026).
  Gartner: open models more verbose and retry more; per-task saving vs frontier
  ~5-6x, not the ~20x the price list suggests
  (https://www.theregister.com/ai-and-ml/2026/11/03/open-models-can-do-90-of-agentic-work-at-a-twentieth-of-the-cost-of-a-frontier-llm-gartner/5307132).

## Moved from post 1 (2026-11-15): company strategies
Steff's note: this section is about cost, not supply risk, so it moves here. Fits
part 3 ("under which conditions each company strategy is rational"). Text as it
was in the post, footnotes included ([^pragmatic] is still also used in post 1).

### How companies optimize AI spend (from post 1)

If the trade-off is real, companies should land in different places. They do. Most figures below are reported by the companies themselves.

| Strategy | Examples |
|---|---|
| **All-in on frontier models** | Cognizant has put more than 25,000 people on Claude.[^cognizant] Deloitte is rolling Claude out to its whole workforce.[^deloitte] Sierra reports 5x more output on some tasks with Claude Code and Codex.[^sierra] |
| **Both at once** | Uber spent its 2026 AI tools budget in four months,[^uber] then moved some tasks to open models and halved its cost per AI session.[^pragmatic] |
| **Open by default, frontier as the exception** | Revolut's default coding model is a self-hosted open model, at a tenth of its former bill.[^revolut] Intercom matched GPT-4.1 on one task with a fine-tuned 14B Qwen model.[^intercom] AT&T routes each task to the smallest model that can do it, with savings of up to 90%.[^att] |
| **Own models** | Pinterest pays less than 8% of the closed-model cost per transaction.[^pragmatic] C3 AI's fine-tuned model beats frontier models on its own software, faster.[^c3] |

Three things stand out:

- **The work that moves is high-volume or specific to the business.** Revolut's everyday coding, Intercom's support pipeline, AT&T's telecom work, C3 AI's own software. The hardest and newest work stays with frontier models.
- **Almost nobody moves completely.** Companies decide per task.
- **The all-in examples are often services firms.** For Cognizant and Deloitte, betting on one provider is partly what they sell.

[^cognizant]: (checked) [Analytics India Magazine, 11 November 2026](https://analyticsindiamag.com/ai-news/cognizant-commits-25000-professionals-to-deploy-claude-in-enterprises)
[^deloitte]: (checked) October 2025. [Anthropic](https://www.anthropic.com/news/deloitte-anthropic-partnership) · [Deloitte](https://www.deloitte.com/us/en/about/press-room/deloitte-and-anthropic-collaborate-to-bring-safe-reliable-trusted-ai-to-commercial-government-organizations.html)
[^sierra]: (checked) Self-reported, January pilot. [Sierra, "AI-pilling our company"](https://sierra.ai/blog/ai-pilling-our-company-lessons-learned)
[^uber]: (checked, secondary; original reporting by The Information) [Fortune, 26 May 2026](https://fortune.com/2026/05/26/uber-coo-ai-spending-tokens-claude-code/) · [Outlook Business](https://www.outlookbusiness.com/corporate/uber-caps-ai-tool-usage-after-exhausting-yearly-budget-in-just-4-months) ($1,500 monthly cap per employee)
[^revolut]: (checked) [AI Engineer talk](https://ai.engineer/talks/Ok6QdZHCcI8-revolut-cut-ai-coding-costs-90-self). The comparison doesn't match usage periods or include the cost of running the service.
[^intercom]: (checked) [Intercom (Fin) research, "David vs Goliath: are small LLMs any good?"](https://fin.ai/research/david-vs-goliath-are-small-llms-any-good/) · [Cost of serving LLMs](https://fin.ai/research/cost-of-serving-llms/)
[^att]: (checked) [AT&T, "The tokenomics equation", 23 July 2026](https://about.att.com/blogs/2026/the-tokenomics-equation.html)
[^c3]: (checked) [C3 AI blog](https://c3.ai/blog/why-c3-ai-fine-tunes-its-own-models-even-with-frontier-ai-this-strong)
[^pragmatic]: (checked) [The Pragmatic Engineer, "Tech companies move to open AI models"](https://blog.pragmaticengineer.com/the-pulse-tech-companies-move-to-open-ai-models/): Uber's cost per AI session down 52%; AT&T 56% cheaper on some tasks for about 2% lower quality; Pinterest CEO: "less than 8% of the cost of comparable closed proprietary models".

## Decisions (round 2)
- Part 1: one question: "When does it pay to own instead of rent?" My own case
  (is investing justified for me) becomes the example in part 4.
- Part 3: scenarios with explicit, marked assumptions (option a). This sets the bar
  open models must reach per task.
- Part 4: the lesson is the quantitative side of "own the core, rent the edge":
  the formula shows where the line between core and edge lies.
- Owning is a fundamentally different investment: a capital investment. More than
  price risk, the core is: a company that doesn't even own its own processes.
  Capital is not better by definition; there are scenarios where renting wins.
- Post 1 and post 2 should be mirror images.

## 1. The question
- When does it pay to own instead of rent?

## 2. The approach
- Cost model, which also defines the criteria I will judge models on.

### Mental model (Steff): filter first, then minimise cost
1. Hard filters: criteria from post 1 that rule options out for a task
   (e.g. control: data may not leave the EU).
2. Cost minimisation among what remains. Criteria that don't filter
   (e.g. access) carry their own cost and enter the formula.
- Suggestion (in discussion): a criterion is not a filter or a cost by nature; it
  depends on the task. The same criterion can be a hard filter for one task and a
  cost for another. A hard filter = an infinite cost.
- Independence: priced as a hedge against price increases (see lesson), not a vague preference.

### Cost model (draft)
Operating cost:
cost per successful task =
  (model cost per attempt + human review time x hourly rate) / success rate
  + (setup/tuning engineering time x hourly rate + fixed costs) / number of tasks
  + risk premium (P(disruption) x switching cost)
- Hourly rate for illustration: EUR 100/h.
- Model cost per attempt by option: tokens x price (pay-per-use); fixed fee / N up to
  the rate limit (subscription); GPU hour / (tasks per hour x utilisation) (renting);
  (depreciation + power + maintenance) / N (ownership).
- Default mapping of the six criteria (can change per task, see mental model):
  control = filter (or premium if it is a preference); performance = filter on
  latency/availability, otherwise success rate; access = cost via success rate and
  review time (a weaker model fails more or needs more checking); effort =
  engineering and review time; continuity = filter (a fallback is required) or risk
  premium; economics = model cost.
- Routing: filter on constraints, pick lowest cost per successful task, keep a fallback.

Investment (fixes "the formula punishes small models"):
- Harness, evals, decomposed processes, gateway = one-off investment that changes
  the parameters of all future tasks (higher success rate for small models, less
  review time, lower switching cost).
- Return ~ (cost per task before - after) x future tasks - investment.
- Decomposing a task raises reliability and lowers cost; small models are bad at
  undecomposed tasks, not at tasks.
- Process knowledge and swappability are capital. Swappability has option value:
  it lets you capture each step on the exponential curve. Grows with volume and
  with the speed of the curve. No single number yet.

### Presentation (agreed)
- Layer 1: mental model in one line + flow:
  task -> hard filters -> remaining options -> lowest cost per successful task -> pick + fallback
- Layer 2: formula in words, as a table with three named parts: running cost,
  set-up cost, risk. No math notation.
- Layer 3: two concrete insights with my own numbers (illustrative until tests run):
  1. Break-even in minutes: Databricks task ~$2.7 model cost; a 10x cheaper model
     saves ~$2.4 = ~1.5 minutes of my time at EUR 100/h.
  2. Chart: cost per task vs number of tasks. Flat line for frontier (no set-up),
     falling curve for open model + harness. Crossing point = when investing pays.

### Rent vs own (core idea from post 1, made calculable)
- Frontier usage = rent (operating cost, no asset, provider sets the price).
- Own stack + knowledge = capital investment (asset that keeps value whichever
  model wins). The investment layer of the formula is where this lives.
- Why renting is so tempting now: it is cheap (subsidised; ~86% of task cost is my
  own time). That is exactly why the capital view is needed.

## 3. What happened
### Worked example: the clinical-trials session (2026-11-08)
Cumulative cost at API list price (Claude Code result events):
| Phase | Wall clock | Cost |
|---|---|---|
| Interview (grilling, Q1-Q24) | 09:31-09:43 | $0.50 |
| Build on Databricks + verify | 09:43-09:55 | ~$2.7 |
| Fabric / Power BI (interview + build) | 09:57-10:18 | ~$3.4 |
| OVHcloud (interview + build + Superset) | 10:27-13:25 | ~$10.3 |
| Total | ~4 h | $16.90 (Opus $16.71, Haiku $0.19) |
- Human time: ~1 hour typing answers (Steff's estimate) = ~EUR 100 vs ~EUR 16 model:
  ~86% human, ~14% model.
- Volume is mostly cache reads (37.6M tokens), not output (~205k): a long agent
  session re-reads its whole context at every step. Argument for decomposition.
- Same task cost ~3x more on OVH than Databricks. Task complexity (less known
  platform, documentation not reachable, Superset error) drives cost.
- One session at list price ~ one month of subscription: subscriptions are cheap
  for heavy agentic work, within limits.
- Under which conditions each company strategy from post 1 is rational.
  TODO: work out per strategy.

## 4. The lesson
- Human time and task decomposition weigh more than the token price.
- One-shots ride on the frontier; systems capture the curve.
- Candidate core: frontier to explore, open models once something becomes repeatable.
- Case for me: if data engineering tasks become my bread and butter, investing in
  open-source technology is justified.
  - Data engineering is, like software engineering, logic-based and regular. Every
    project needs pipelines, CI/CD etc., likely doable by relatively simple models.
    Regulatory data contracts will follow a fixed structure. That is a lot of work.
  - Understanding and verification are probably more complex.
  - Hypothesis, to be confirmed by the tests that still have to run.
    Suggestion: show it as a routing table by task type (hypothesis), checked later.
  - Counter-argument "frontier gets cheaper, so the gap shrinks": can't count on it.
    Compute commitments and training costs must be recouped; "Uber effect":
    dominate the market, then raise prices. A diverse field is the only way to keep
    token prices low.
    Suggestion: independence then is a hedge against price increases (risk
    premium / option value), not just a preference. Note: keeping the field diverse
    is a public good; for one buyer the rational part is the hedge.
  - Client demands: open/European models, possibly local or edge processing, are a
    legitimate hard filter. The use case must be able to handle it.
    Chain (suggestion): filter forces small models -> small models only work on
    decomposed tasks -> investing in harness/evals becomes a precondition, not a choice.
- Suggestion: "pin the formula, not the answer".

## 5. What next
- Planned post: turning repeated one-shots into well-understood systems with
  harness, evals etc. "The real gain comes when you have that."
- TODO(steff): open question.
