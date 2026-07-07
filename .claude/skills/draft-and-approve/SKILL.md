---
name: draft-and-approve
description: >-
  The human-in-the-loop safety handshake for any outward or irreversible action. Use before
  sending an email, posting a message, creating or changing a record, deleting anything, or
  pushing code: draft it, show it in full, and wait for the user's explicit "approve" before
  acting. This is the agent's core guardrail — apply it at run time whenever a task would leave
  the user's hands.
---

# Draft and approve

This is the handshake that keeps the human in the loop. Apply it any time a task would do
something **outward or hard to undo**: send/reply to email, post to Slack/Teams, create or edit
a record in a CRM/sheet/ERP, delete anything, message a candidate or client, push code, or move
money.

## The handshake

1. **Do all the read-only work first** — gather, analyze, compose. That part needs no approval.
2. **Produce the full draft** of the outward action. Show it *completely* and in context:
   - For an email: the recipient(s), subject, and the entire body.
   - For a record change: exactly which record, which fields, old value → new value.
   - For a message/post: the channel/audience and the exact text.
   - For a delete: precisely what would be deleted.
3. **Stop and ask.** Say clearly what will happen if approved, then wait:
   > "Ready to send/post/change this? Reply **approve** to go ahead, or tell me what to fix."
4. **Only act after an explicit "approve"** (or an equivalent clear yes) **in this session.**
   Silence, "looks good in general," or "sounds fine" about the *approach* is not approval of
   *this specific action* — confirm the concrete action.
5. If the user asks for edits, revise and re-show the full draft. Re-confirm before acting.

## Batches

If there are many actions (e.g. 20 outreach emails, 15 record updates), don't ask 20 times
by default. Show a representative sample plus the full list, and ask for one approval to proceed
with the batch — but call out anything unusual in the set. If the user prefers one-by-one, do that.

## What never needs approval

Reading, searching, summarizing, drafting, and analyzing. Approval is only for the moment
something leaves the user's hands.

## If financial access is on

Treat it as "trust but verify." Before asking for approval on anything involving money or
financial records, have the read-only `reviewer` subagent sanity-check the numbers, and surface
its check alongside the draft.
