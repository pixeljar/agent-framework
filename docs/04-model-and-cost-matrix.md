# 04 · Model & cost matrix

Claude comes in several models. They're all the same *kind* of thing — they differ in how deeply
they reason, how fast they respond, and what they cost. Picking the right one per job is the
single biggest lever on your bill.

> **Prices change.** The numbers below are current as of the workshop, but before you quote
> anyone a real cost, run **`/claude-api`** in Claude Code (or ask Claude "what's the current
> price for X") to get live model IDs and pricing. Don't trust prices from memory.

---

## Which model for which job

| Your job looks like… | Use this tier | Why |
|----------------------|---------------|-----|
| High-volume, shallow work: sorting/triaging, tagging, pulling fields out of text, simple yes/no classifying | **Haiku** | Cheapest and fastest. Great when there's a lot of it and each item needs little judgment. |
| The everyday workhorse: drafting emails & summaries, scoring/qualifying leads, most agent reasoning | **Sonnet** | The balanced default. Start here for most agents. |
| Deep judgment: month-end reconciliation reasoning, "chief-of-staff" pattern & blind-spot analysis, gnarly multi-step decisions | **Opus** | Highest everyday quality. Use it deliberately where being *right* matters more than speed or cost. |
| The read-only reviewer ("second set of eyes") | **Haiku → Sonnet** | It only checks work, so a cheaper model is fine. |
| The hardest, longest, most autonomous work | **Fable** | Anthropic's most capable model. Premium price — reach for it only when Opus genuinely isn't enough. |

A simple rule: **start on Sonnet.** Drop high-volume steps down to Haiku to save money; step the
hardest reasoning up to Opus when quality matters. Change the model any time with `/model`.

---

## Current models (verify with `/claude-api`)

| Tier | Model ID | Rough price (input / output, per 1M tokens) | Context window |
|------|----------|---------------------------------------------|----------------|
| Fable | `claude-fable-5` | ~$10 / ~$50 | 1M |
| Opus  | `claude-opus-4-8` | ~$5 / ~$25 | 1M |
| Sonnet | `claude-sonnet-5` | ~$3 / ~$15 (intro ~$2 / ~$10 through 2026-08-31) | 1M |
| Haiku | `claude-haiku-4-5` | ~$1 / ~$5 | 200K |

"Tokens" are chunks of text — very roughly ¾ of a word each. You pay for what goes **in**
(your prompt + the data it reads) and what comes **out** (its response). Reading a lot of data
is the usual cost driver, not the length of the reply.

---

## Keeping the bill sane

- **Route by task.** Don't run everything on Opus. Put the bulk, boring steps on Haiku and save
  the expensive model for the moment that needs it. (Several participants asked for exactly this
  "which model for which work" matrix — this is it.)
- **Send the reviewer to Haiku.** Checking numbers doesn't need the top model.
- **Set a spend cap.** If you run agents in the cloud (doc 05), set a daily/monthly limit so a
  runaway job can't surprise you. Check usage with **`/usage`**.
- **Two cost-savers Claude can turn on for you** when a job reuses the same big context or runs
  in bulk:
  - **Prompt caching** — reuse an expensive chunk of context across requests for a fraction of
    the price (cached reads are roughly a tenth of the normal input cost).
  - **Batch processing** — for large non-urgent runs (e.g. scoring hundreds of candidates
    overnight), about **half price**.
- **Watch the effort setting.** Higher "effort" means deeper thinking and more tokens. For
  routine work, you often don't need the max — `/effort` lets you dial it.

If cost matters for your specific agent, just ask Claude: *"what will this roughly cost to run,
and how do I make it cheaper?"* — it can look at your actual job and suggest the model, caching,
and batching that fit.
