# 07 · Checking it stays right

You built an agent, watched it work, and trusted it. Then something changes — you swap to a newer
Claude model, reword its playbook, or add a step. A change that *looks* harmless can quietly make
the output worse, and you won't notice until it's already gone out. The fix is small: keep a few
examples where you already know the right answer, and re-check them after any change.

> **You direct Claude. You stay in the loop.** "Is it still right?" is your question to ask, not
> the agent's to answer for you. This page is how you get proof instead of a hunch.

---

## Why bother — the model won't sit still

Anthropic ships new models regularly, and you'll *want* to move: a cheaper one to save money, a
faster one for speed, a smarter one for a hard job. Every one of those swaps is a chance for the
output to change in a way you didn't intend. The same goes for editing the agent's instructions.
A handful of known-good examples turns "I *think* it's still fine" into "I *checked*."

## Keep a few "known-good" examples

A **golden set** is 2–3 real cases where you already know what a good result looks like. That's
it. Don't build a giant test suite on day one — a few good examples catch most drift. They live in
an `evals/` folder next to your agent, one small file each.

One rule: **keep them free of sensitive data.** No SSNs, no full account numbers, no health data
in your examples — redact them or use a trimmed, made-up version. The examples get re-read every
time you check, so the less sensitive data in them, the better (this is guardrail 3, still on).

## Two kinds of "right"

How you check depends on the job:

- **Concrete answers** — a total, a number, which record, a yes/no. You check these **exactly**:
  the new output either matches the known-good value or it doesn't. Month-end reconciliation is
  the clean case — the figure is right or it's wrong, no debate.
- **Judgment calls** — the tone of a draft email, which candidates made the shortlist, what
  counts as a "red flag." You check these **by feel**: put the new result next to the known-good
  one and see what changed. The read-only **reviewer** subagent is a good second set of eyes here.

Most agents are a mix. Knowing which kind each example is tells you how to score it.

## Run it after any change

Re-run the agent on your saved examples and compare each to its known-good answer. This is always
a **dry run** — it drafts the output and compares; it never sends, posts, or writes to a real
record. In Claude Code, just run **`/check-it`** — it walks the whole loop: save the examples the
first time, then re-run and report **✓ still right / ~ drifted / ✗ worse** for each one, and hands
the decision back to you.

## Before you swap models

This is the moment evals earn their keep. Before you move an agent onto a new model:

1. Run the golden set on your **current** model — that's your baseline.
2. Switch with `/model` (or change the effort with `/effort`).
3. Run the golden set again on the **new** model.
4. Compare. Keep the new model **only if it holds up.** (Model tiers and prices: doc 04.)

Same idea, smaller: after any edit to the playbook, re-run the set before you trust the change.

## When it's wrong, save that as a test

The best golden examples come from real mistakes. Any time the agent gets something wrong, save
that case into `evals/` as a new example with the *right* answer. From then on, the check catches
it — that particular mistake can never quietly come back. Your set gets sharper the more you use
the agent, exactly where it's been weak.
