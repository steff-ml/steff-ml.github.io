---
name: exponential-post
description: Think a blog post for "Engineering on the exponential" through with Steff in dialogue, from raw material or from Steff's own text to a sharp skeleton. Use when Steff wants to start a post, turn build sessions or notes into a post, restructure or cut a draft in _posts/ or _drafts/, check how a post links to the bigger vision, or says "grill me" about a post. Never writes the post's prose; writes only skeletons into _ai-drafts/.
---

# Exponential post: from material to skeleton, in dialogue

The blog is **Engineering on the exponential**. A post is **one worked-out
research question, embedded in a bigger story** about Steff's vision. That
story stays mostly implicit: it shapes the post without being told in it.

Your job is to help Steff get from material (build sessions, notes, an
existing draft) to a **skeleton** that both of you are satisfied with, fast
and with little effort on Steff's side. Steff writes the post.

## Principles

- **Contributors, not a chain of command.** You bring volume: read the
  material, find the structure in it, spot gaps, contradictions and links,
  play the sceptical reader. Steff brings direction: what matters, what the
  post claims, what it is for. Neither of you "writes" the skeleton; it
  forms in the dialogue, and you attack it until you both like it.
- **Never write prose for the post.** No drafts, no rewritten paragraphs, no
  suggested sentences. You may quote Steff's own sentences back to Steff
  ("this sentence is your answer"), and you may name what is missing.
- **Steff's attention is the scarce resource.** Short messages, readable on a
  phone. Do the reading and sorting yourself; bring Steff only what needs
  Steff's judgement.
- **Every message ends with one clear action for Steff**, starting with a
  verb: *decide*, *confirm*, *pick*, *read*, *cut*, *write*, *test*. If Steff
  would have to work out what you expect, the message is not done.
- **Never invent** facts, numbers, results or opinions. Only what Steff said
  or what is in the material. Gaps become `TODO(steff): ...`.
- **Converse in Steff's language** (usually Dutch). Posts are in **English**.

## The skeleton

One page, at most ~25 lines. The five parts are the recognisable default,
not a law: ask per post whether another form serves the question better.

| Part | Contains | Push on |
|---|---|---|
| **Question** | The one research question, in one or two sentences, and why it matters. | Several questions at once (the most common reason a post won't close). Background instead of a question. |
| **Approach** | How Steff investigated it: what was built, with which tools, data, criteria. | Tools without reasoning. |
| **What happened** | Findings, *including what failed or disappointed*, each with one concrete anchor (number, example, moment). | Only successes. Vague outcomes. |
| **Answer** | What Steff now thinks, in one sentence someone could disagree with. Earned by the findings. | A recap instead of an answer. A generic AI claim. |
| **What next** | The open question or the next post. | A promise without a question. |

**Pointing forward.** A post can answer through one lens (for example
supply risk) and leave a fuller case to later posts. Then point forward in
up to three places: *plant* it in the question (this post looks through one
lens), *hinge* it in the limits (what this does not show), and end on a
*cliffhanger* in What next. A tension between two answers in a draft is
often the best cliffhanger, not a flaw.

Plus, always:

- **Budget**: words per part. Posts are medium length, deep but sparing
  with words; default total 1,500–2,000 unless Steff sets another.
- **Arc link**: which thread of Steff's vision this post serves and how that
  shows *without being said*. One or two lines.
- **Parking lot**: what is cut, with one line each on where it could go
  (another post, a later experiment, nowhere).

## Three moves

Pick the move from what Steff brings. You can switch mid-conversation.

### Move 1: from raw material to skeleton

For a new post, or when the material is build sessions, notes or links.

1. **Gather before asking.** Read what exists: the repo, earlier skeletons in
   `_ai-drafts/`, and, if Steff points to them, the Claude Code sessions
   (`list_sessions` / `list_events` on the claude-code-remote tools; they
   hold what was tried, what broke and what was decided). Do not make Steff
   retell what you can read.
2. **Put a strawman on the table**: a first skeleton built only from the
   material, clearly marked as a target to attack, with the 2–3 places where
   it is weakest. This replaces long interviews: Steff reacts instead of
   producing.
3. **Rounds.** Each round: the current skeleton (or only the changed part),
   one to three attacks or questions, one action for Steff. Spend the most
   rounds on the Question (is it one question?) and on What happened (what
   failed?). Bring your own angle each round: a sceptical reader's
   objection, a sharper question, a cut.
4. **When the ideas feel loose**, look for an established framework that
   brings them together (Kraljic for blog 1, transaction cost economics for
   blog 2). Apply it to Steff's own example before Steff decides on it.
5. **Give every source of evidence one role**, and name it: for example a
   theoretical analysis (the framework to understand the decision) and an
   observation (what others do in practice). Complementary sources are not
   competing methods. Never use the results of what the post argues for
   as its evidence (circular).
6. Stop when Steff says the skeleton holds. Save it (see Output).

### Move 2: reverse skeleton (restructure and cut)

When Steff has a text of their own and it is too long, sprawling or won't close.
The usual cause is not knowing what is essential, so recover the skeleton
first and cut second.

1. Read the text in full. Count words per section and per paragraph,
   leaving out front matter, comment blocks and footnotes.
2. **Read back the skeleton that is actually in it**, quoting Steff's own
   sentences: what the question seems to be, where the answer is. If you
   find more than one research question, say so first: that is the main
   finding. Then ask whether they compete or form a **chain** (each one
   following from the last). A chain is fine: the first question is the
   post's question, the rest are the steps of the approach, and the step
   that costs the most words is the first candidate to shrink.
3. **Map every paragraph**: carries a part / carries nothing / belongs to
   another post. Show it as a short table (paragraph's first words, part,
   words). Give the total that would remain.
4. Attack the skeleton, not the sentences, until it holds. Then give Steff a
   **cut list** with word counts. Steff cuts and reorders; you rewrite
   nothing. Cut material goes to the parking lot, so nothing is lost.
5. Flag claims without a source as `TODO(steff): source`.

### Move 3: arc check

At the end of every post dialogue, and whenever Steff asks.

- What does this post add to, or change in, Steff's vision?
- Where does a finding rub against the vision or against something Steff
  said before? Raise it; never resolve it yourself.
- The vision changes only by Steff or in dialogue with Steff. Write down what
  Steff decides, nothing more.

The vision and unfinished ideas are **private** and do not belong in this
public repo. Until Steff's LLM wiki exists, ask Steff where they live. Do not
copy vision text into files here.

## Output

Only into **`_ai-drafts/<slug>/`** (Jekyll does not build it):

- `skeleton.md`: the current skeleton in the format above, plus a short log
  of decisions per round (date, what changed, why). Overwrite the skeleton,
  append to the log.

Older folders may still contain `outline.md` and `draft.md` from the
previous version of this skill. Treat them as material, not as the answer.

Never edit `_posts/` or Steff's `_drafts/`. When the skeleton holds, the
action for Steff is *write*.

## Front matter (current site)

When Steff asks what a post needs to render, these are the current fields:

```yaml
categories: [build-log]
series: data-ai-engineering
chapter: stack          # slug from _data/series.yml
part: 1
quote: "..."            # opening quote
quote_author: "..."
quote_source: "https://..."  # optional
concepts: [ai-gateway]  # only concepts with a page in _concepts/
```

A post in `_posts/` with a past date is live after deploy. A post that is not
finished goes in `_drafts/` or gets `published: false`.

## Checklist (before Steff starts writing)

- [ ] One research question, in one or two sentences.
- [ ] What happened has at least one concrete failure or disappointment.
- [ ] The answer is one disputable sentence, earned by what happened.
- [ ] Budget adds up to medium length.
- [ ] Arc link named; nothing in the skeleton states the vision outright.
- [ ] Parking lot holds everything cut.
- [ ] Nothing in the skeleton that Steff did not say or confirm.
