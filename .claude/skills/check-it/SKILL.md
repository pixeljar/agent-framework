---
name: check-it
description: >-
  Check that an agent still does its job right — especially after you change its playbook or swap
  the Claude model. Saves a few known-good examples (a "golden set"), re-runs the agent on them,
  and reports what stayed right and what drifted. Use before trusting a change, before a model
  upgrade goes live, or whenever the user wants proof the agent is still correct. Runs as a dry
  run — it drafts and compares, it never sends or writes to real records.
---

# Check it — does the agent still do its job right?

A change that *looks* fine can quietly make an agent's output worse — a new model, a tweaked
playbook, a reworded instruction. This skill catches that. You keep a few examples where the
right answer is already known, re-run the agent on them after a change, and see what drifted.
Keep it small: **2–3 examples is enough to start.**

> This is a **dry run.** Never send, post, or write to a real record while checking — draft only,
> read from copies. An eval that emails a real client is not an eval.

## Two kinds of "right" (decide this per example)

- **Concrete answers** — a number, which record, a yes/no, a total. These you check **exactly**:
  did the new output match the known-good value? (Month-end reconciliation totals are the clean
  case — the number is either right or it isn't.)
- **Judgment calls** — the tone of an email, which candidates made the shortlist, what counts as
  a "red flag." These you compare **by feel**: put the new output next to the known-good one, and
  flag what changed. Hand it to the read-only `reviewer` subagent for a second opinion.

Most agents have some of both. Mark each example so you know how to score it.

## Step 1 — build the golden set (first time only)

Start from the agent's own **`## What "good" looks like`** section (set during `/build-agent`,
question 6) — that rubric is exactly what a golden example checks against. Then ask the user for
**2–3 real cases where they already know the right answer.** For each, capture:

- **The situation / input** — the data or request the agent would work from.
- **What a good result looks like** — for a concrete case, the exact expected value(s); for a
  judgment case, a known-good example output plus the *must-haves* and *must-never-happens*.

Save each as a small file under `evals/` (create the folder if needed), e.g. `evals/01-close.md`.
**Keep these PII-free** — redact SSNs, full account numbers, and health data; use a trimmed or
synthetic example if the real data is sensitive (guardrail 3 still applies here).

## Step 2 — run the check (any time after)

Re-run the agent on each saved case, **read-only**. Produce the output it *would* have produced —
but stop at the draft; do not take the outward action. Do this for every case in `evals/`.

## Step 3 — compare and report

For each case, score against its known-good answer:

- **Concrete** → exact-match the value(s). Right or wrong, no interpretation.
- **Judgment** → side-by-side the new output and the known-good one; list what changed; run the
  `reviewer` subagent if it's a close call.

Report one line per case, then a verdict:

- **✓ still right** — matches the known-good answer.
- **~ drifted** — changed, but might be fine; show what changed so the user can judge.
- **✗ worse** — misses a must-have, hits a must-never, or the number is wrong.

## Step 4 — the user decides

Present the report and **stop.** The user decides whether to accept the change (or the new model).
Never auto-accept a change because the eval "mostly passed" — surface the drift and let them call it.

## Before a model swap (the migration case)

This is the highest-value moment to run it. Newer/cheaper/faster models appear regularly; before
you move an agent onto one:

1. Run the golden set on the **current** model — confirm it still passes (your baseline).
2. Switch with `/model` (or change `/effort`).
3. Run the golden set again on the **new** model.
4. Compare the two reports. Keep the new model **only if it holds up.** See `docs/04-model-and-cost-matrix.md`.

## When it's wrong, save that as a test

Any time the agent gets something wrong in real use, turn that case into a new golden example.
That mistake can now never quietly come back — the check will catch it next time.
