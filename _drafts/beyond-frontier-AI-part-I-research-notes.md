# Research notes: Beyond frontier AI, part I

Working notes for `2026-09-25-beyond-frontier-AI-part-I.md`. Not a post (no front matter, so Jekyll skips it).

Legend: ✅ checked via search on 2026-09-25 · 📚 from memory, check the link before citing · 👤 provided by me, not checked

---

## 1. Decisions already made

- **Reframe around independence, not open source.** The stack mixes open and proprietary (Databricks, Scaleway, LM Studio is freeware, not OSS). Change the title/excerpt from "open-source AI infrastructure" to something like "flexible, vendor-independent."
- **Fix the excerpt/tags.** The excerpt promises "data products," but the post never mentions them. Add a data-product example or change the excerpt.
- **Subscription argument stays.** Subscription for exploration and building; compound system for operational workloads. Add one line noting that Pro/Max subscriptions can no longer be used in third-party harnesses (see §2), and use that as evidence for dependency risk, not as a caveat.
- **Soften "none of these risks are theoretical."** Mark which have happened and which are plausible. The export-ban scenario hasn't happened; the Pentagon case is the real-world example.
- **Open weights and bias: framing.** No model is value-neutral; trained-in behaviour goes wherever the weights are hosted. The advantage of open weights is that you can measure it (evals) and change it (fine-tuning). Don't claim open models are more dangerous.

---

## 2. Evidence for the risks in the intro

| Risk | Evidence | Status |
|---|---|---|
| Loss of access (political) | Pentagon designates Anthropic a "supply chain risk" (Feb/Mar 2026); appeals court upheld it on 2026-09-25. [Axios](https://www.axios.com/2026/02/27/anthropic-pentagon-supply-chain-risk-claude) · [Mayer Brown](https://www.mayerbrown.com/en/insights/publications/2026/03/pentagon-designates-anthropic-a-supply-chain-risk-what-government-contractors-need-to-know) · [CNBC](https://www.cnbc.com/2026/09/25/pentagon-anthropic-ai-risk-appeals-court.html) | ✅ |
| Loss of access (vendor policy) | Anthropic blocks subscription OAuth in third-party harnesses incl. OpenCode (enforced Jan 2026, made explicit Feb 2026, full block Apr 2026). [VentureBeat](https://venturebeat.com/technology/anthropic-cracks-down-on-unauthorized-claude-usage-by-third-party-harnesses) · [AlternativeTo](https://alternativeto.net/news/2026/2/anthropic-officially-bans-using-subscription-authentication-for-third-party-claude-use) | ✅ |
| Performance degradation | Anthropic postmortems: infra bugs [Aug/Sep 2025](https://www.anthropic.com/engineering/a-postmortem-of-three-recent-issues); defaults/system prompt changes in [Mar–Apr 2026](https://www.anthropic.com/engineering/april-23-postmortem). Bugs and harness config changes, **not** secret model swaps; word carefully. | ✅ |
| Model drift (general) | Chen, Zaharia & Zou, "How is ChatGPT's behavior changing over time?" [arXiv 2307.09009](https://arxiv.org/abs/2307.09009) | 📚 |
| Data use | Consumer plans (Free/Pro/Max incl. Claude Code) used for training unless you opt out, 5-year retention; API/commercial excluded. [Anthropic](https://www.anthropic.com/news/updates-to-our-consumer-terms) | ✅ |
| Cognitive debt | The term comes from Kosmyna et al., MIT Media Lab. [arXiv 2506.08872](https://arxiv.org/abs/2506.08872) | ✅ |
| Cognitive debt (dev productivity) | METR 2025 RCT: experienced OSS developers slower with AI while believing they were faster | 📚 find link |

---

## 3. Evidence that open models are good enough

- **Epoch AI:** open-weight models lag the closed frontier by about 4 months (since Jan 2026; about 3 months over 2023–2025). [Epoch](https://epoch.ai/data-insights/open-closed-eci-gap) ✅
- **Dreamforce:** business leaders say older models are enough. [CNBC](https://www.cnbc.com/2026/09/18/at-dreamforce-business-leaders-say-older-ai-models-are-enough.html) 👤 (blocked my fetch)
- **Combined argument:** the best open model is a few months behind, and many businesses say last generation is enough.

---

## 4. Sources that push back (address these in the post)

1. **Enterprise open-model share is falling.** Menlo Ventures 2025: 19% → 11%; Anthropic about 40% of enterprise LLM spend; Chinese open models about 1%. [Menlo](https://menlovc.com/perspective/2025-the-state-of-generative-ai-in-the-enterprise/) ✅
   → Reply: adoption lags capability, and lock-in is part of why.
2. **LiteLLM supply-chain attack.** v1.82.7/1.82.8 on PyPI, 2026-03-24, credential stealer via compromised Trivy in CI. [LiteLLM](https://docs.litellm.ai/blog/security-update-march-2026) · [Datadog](https://securitylabs.datadoghq.com/articles/litellm-compromised-pypi-teampcp-supply-chain-campaign/) · [Snyk](https://snyk.io/blog/poisoned-security-scanner-backdooring-litellm/) ✅
   → Independence means taking on this risk yourself: pinned versions, lockfiles, isolated secrets.
3. **Bitter Lesson.** Sutton 2019: general methods plus compute beat hand-built structure. [Link](http://www.incompleteideas.net/IncIdeas/BitterLesson.html) 📚
4. **Hosting in an EU region ≠ sovereignty.** Microsoft France told the French Senate under oath it "cannot guarantee" EU data is safe from US CLOUD Act requests. [The Register](https://www.theregister.com/2025/07/25/microsoft_admits_it_cannot_guarantee/) · [Forbes](https://www.forbes.com/sites/emmawoollacott/2025/07/22/microsoft-cant-keep-eu-data-safe-from-us-authorities/) ✅
   → Actually *supports* the case for Scaleway / OVH / Mistral.
5. **Total cost of ownership.** GPUs, ops time, patching. Needs one honest sentence; no source needed.

---

## 5. Open weights: bias and fine-tuning

**Bias travels with the weights**
- CrowdStrike (Nov 2025): DeepSeek-R1 produces up to ~50% more severe vulnerabilities when prompts mention Tibet/Uyghurs/Falun Gong, even on unrelated coding tasks. [CrowdStrike](https://www.crowdstrike.com/en-us/blog/crowdstrike-researchers-identify-hidden-vulnerabilities-ai-coded-software/) · [The Hacker News](https://thehackernews.com/2025/11/chinese-ai-model-deepseek-r1-generates.html) ✅
  → To check: did they test the raw model or DeepSeek's app?
  → Good hook for the evals section.
- Avoid the NIST CAISI DeepSeek report as the main source: it comes from a US government agency with an interest in the result and is about "danger," not hosting.

**Fine-tuning fixes it**
- Perplexity R1 1776: DeepSeek-R1 post-trained to remove CCP censorship, performance kept, weights published. [The Decoder](https://the-decoder.com/perplexity-ai-removes-chinese-censorship-from-deepseek-r1/) · [Computerworld](https://www.computerworld.com/article/3829462/perplexity-releases-censorship-free-variant-of-china-s-deepseek-r1-ai-reasoning-model) ✅
- Cheap: LoRA [2106.09685](https://arxiv.org/abs/2106.09685), QLoRA [2305.14314](https://arxiv.org/abs/2305.14314) 📚
- Guardrails can be removed surgically: Arditi et al., "Refusal … Mediated by a Single Direction," NeurIPS 2024. [2406.11717](https://arxiv.org/abs/2406.11717) ✅

**Caveats**
- Fine-tuning can weaken safety, even on harmless data: Qi et al. 2023 [2310.03693](https://arxiv.org/abs/2310.03693) 📚
- Backdoors can survive later training (applies to *any* model you didn't train): Sleeper Agents [2401.05566](https://arxiv.org/abs/2401.05566) 📚

**Draft wording**
> No model is value-neutral: every model carries the choices of whoever trained it, and those choices travel with the weights wherever they're hosted. The difference is what you can do about it. With a frontier model, you get whatever the lab decides and can change it only within the limits of prompting. With open weights, you can measure the behaviour with your own evals and, if you don't like it, fine-tune it out, as Perplexity did with DeepSeek-R1.

---

## 6. Frontier models via cloud partners (Bedrock / Vertex / Azure / Databricks)

- AWS runs Claude on its own infrastructure with the same weights; Anthropic staff have no access to it. [Claude docs](https://platform.claude.com/docs/en/build-with-claude/claude-in-amazon-bedrock) · [AWS](https://aws.amazon.com/bedrock/anthropic/) ✅
- **But licensed, not owned.** It protects against: Anthropic service outages, suspension of your Anthropic account, data leaving your cloud. It does **not** protect against Anthropic bankruptcy (weights become an asset in bankruptcy; whether licenses survive is a legal question).
- Anthropic commits to preserving the weights of released models "for the lifetime of the company": preservation, not guaranteed access. [Anthropic](https://www.anthropic.com/research/deprecation-commitments) ✅
- **Line:** "Multi-cloud hedges operations; only open weights hedge existence."

---

## 7. Missed opportunities to add

- **Fixed weights prevent silent degradation.** A self-hosted model can't change underneath you. The strongest answer to "operational variance." Contrast with the April 2026 postmortem.
- **Evals make switching safe.** Link the monitoring section back to the risks: without evals, "swap in one line of config" is a leap of faith.
- **Portable standards.** MCP → Linux Foundation Agentic AI Foundation, co-founded by Anthropic/OpenAI/Block. [LF](https://www.linuxfoundation.org/press/linux-foundation-announces-the-formation-of-the-agentic-ai-foundation) · [MCP blog](https://blog.modelcontextprotocol.io/posts/2025-12-09-mcp-joins-agentic-ai-foundation/) ✅. Agent Skills is an open spec, adopted by OpenAI, Microsoft et al. [agentskills.io](https://agentskills.io/home) · [GitHub](https://github.com/agentskills/agentskills) · [VentureBeat](https://venturebeat.com/technology/anthropic-launches-enterprise-agent-skills-and-opens-the-standard) ✅ → independence at the tool/skill layer.
- **Risk → mitigation → cost table** at the end. Currently guardrails, cognitive debt, data leakage and privacy lack a real answer.
- **One worked example:** a data-product task split into its deterministic and LLM parts. Also fixes the excerpt mismatch.

---

## 8. Sources for the missing components

| Component | Sources | Status |
|---|---|---|
| Compound AI (concept) | Zaharia et al., [BAIR 2024](https://bair.berkeley.edu/blog/2024/02/18/compound-ai-systems/) | 📚 |
| Small models are enough | Belcak et al. (NVIDIA), [2506.02153](https://arxiv.org/abs/2506.02153) | 📚 |
| Deterministic workflows > agents | Anthropic, [Building effective agents](https://www.anthropic.com/engineering/building-effective-agents) | 📚 |
| Retrieval / vector DBs | Lewis et al., RAG [2005.11401](https://arxiv.org/abs/2005.11401); Anthropic, [Contextual Retrieval](https://www.anthropic.com/news/contextual-retrieval) | 📚 |
| Structured outputs | Outlines [2307.09702](https://arxiv.org/abs/2307.09702); XGrammar [2411.15115](https://arxiv.org/abs/2411.15115) | 📚 |
| Pipeline optimization | DSPy [2310.03714](https://arxiv.org/abs/2310.03714) | 📚 |
| Evals | Hamel Husain, [Your AI Product Needs Evals](https://hamel.dev/blog/posts/evals/) | 📚 |
| Observability | [OpenTelemetry GenAI semantic conventions](https://opentelemetry.io/docs/specs/semconv/gen-ai/) | 📚 |
| AI gateway (Databricks) | [Unity Gateway](https://www.databricks.com/product/artificial-intelligence/unity-gateway) | ✅ |

---

## 9. Small fixes in the draft

- `lama.cpp` → `llama.cpp`
- `API's` → `APIs` (throughout)
- "wire applications … too" → "to"
- `unuseable` → "unusable"
- "Foundational Model API's" → "Foundation Model APIs"
- Empty `-` bullet under "Price hikes"
- Harness names: Qwen Code and pi exist; "DeepSeek harness" unconfirmed. Consider Codex CLI, Gemini CLI, Aider, Cline, Goose.
- LM Studio is freeware, not open source
- Fill in the `<repo>` placeholder in the conclusion

The biggest remaining gap is your own evidence. Once the points we discussed are sharpened, the post's argument will be well sourced, but most of it could be written by anyone who reads the news. What only you can add is what happened when you actually ran this stack. In order of impact:

1. Add one measured comparison. Take a real task from your data work, run it on Claude and on your compound setup (for example an open model via Scaleway, plus deterministic code), and report quality on a small eval set, cost per run, and latency. Even 20 test cases is enough. Three things follow:

It proves the "frontier-level results with simpler models" claim instead of asserting it.
It serves as the worked example we discussed and fixes the excerpt mismatch.
It shows evals in action rather than just describing them.
If the results are mixed, publish them anyway. Honest, mixed results persuade more than a clean win.

2. Decide who the post is for. It currently switches between a consumer view (€20 Pro plan, "anyone can build apps") and an enterprise view (compliance, mission-critical processes). Your distinctive angle is the entrepreneur: someone small enough that a vendor decision can sink them, but serious enough to care about operations. Write for that reader and cut what doesn't serve them.

3. Replace the catalogue with opinions. The components section lists options, and readers can get lists anywhere. For each layer, say what you use, why, and what you tried and dropped. The OpenCode line ("intend to run a few experiments") is a good example of a gap: the post is stronger if you've already run at least one of them.

4. Give readers a decision rule. You end with "frontier models have to earn their place" but don't say how they earn it. A simple rule is what readers will remember and share, for example a 2×2 of how critical the task is against how sensitive the data is, with "frontier via subscription", "frontier via API or cloud partner", "open weights hosted in the EU" and "self-hosted" in the four quadrants.

5. Open with a story, not your fandom. "I love Claude" is a slow start. Open with a concrete moment, such as the day Anthropic blocked OpenCode, or the Pentagon designation, and what it would have meant if your process depended on it. Your appreciation for Claude can follow, and it makes the "but" land harder.

6. Admit where frontier models still win. You mention in the intro that they're strongest on "complex tasks with vague descriptions", then drop the point. Say plainly which tasks you still send to Claude and why. It makes the rest of the argument more credible.

7. Smaller things:

A diagram of your stack (gateway → models → harnesses → monitoring) would replace a lot of text.
Make the repo link useful: a docker-compose file or config someone can run in an afternoon.
Add an "as of September 2026" note, because this landscape changes monthly.
Rewrite the bullets as prose. The outline reads like notes, and prose will force you to connect the risk and solution sections.
If you only do one, do #1. It turns an opinion piece into a field report, and it sets up part II naturally.