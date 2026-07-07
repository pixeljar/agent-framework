# 03 · Guardrails

Four safety defaults ship turned **on**. You don't have to set them up — they're already in
`CLAUDE.md` and `.claude/settings.json`. This page explains what each one does and *why*, in
plain English, so you can decide when (and whether) to loosen it.

The whole idea, in the workshop's words: **You direct Claude. You stay in the loop.** As you
give an agent more independence, guardrails matter **more**, not less.

---

## 1. Human-in-the-loop — "draft, don't send"

**What it does.** The agent does all the reading, thinking, and drafting on its own — but
anything that *leaves your hands* (an email sent, a record changed, a message posted, a file
deleted) stops and waits for you to say **"approve."**

**Why.** This is the difference between a helpful assistant and an unsupervised one. You get the
speed of automation and the safety of a human check at the exact moment it matters. Every
participant in the workshop asked for this, in their own words — "I wanna be in the loop,"
"draft-only," "trust but verify," "reviewers, not doers."

**How to loosen it (later, deliberately).** Once you've watched an agent do a specific action
correctly a few times, you can tell it to stop asking *for that specific, low-risk action* —
e.g. "you can post the daily summary to my private Slack without asking, but still draft
client emails for me." Loosen one action at a time; never flip it all off at once.

---

## 2. Least privilege — only what the job needs

**What it does.** The agent can only reach the apps you explicitly connected, and it leans on
**reading** rather than **writing**. It won't reach for a tool "just in case."

**Why.** A tool that isn't connected can't be misused. Reading a CRM is far lower-risk than
writing to it; the narrowest access that gets the job done is the safest. If a new task needs a
new app, you add it on purpose — you're never surprised by what the agent can touch.

**In practice.** Run `/permissions` and `/mcp` any time to *see*, in plain view, exactly what
your agent can reach. If something's there that shouldn't be, remove it.

---

## 3. Keep PII and secrets out of reach

**What it does.** The agent won't read your `.env` file or anything in a `secrets/` folder, and
it's instructed to keep sensitive personal data — full Social Security numbers, full bank/account
numbers, health data — out of the conversation entirely.

**Why.** The less sensitive data an agent ever sees, the less there is to worry about — for
privacy, for compliance, and for your own peace of mind. Passwords and keys live in your local
`.env` (never shared, never committed) and are referenced by name, so the agent uses them to
sign in without ever *seeing* them.

---

## 4. No financial access by default

**What it does.** The agent will **not** connect to or act on money systems — QuickBooks, bank
feeds, payroll, Dynamics finance — unless you explicitly turn it on during `/build-agent`.

**Why.** Financial systems are the highest-stakes, least-reversible things you could hand over.
Several people drew exactly this line ("wouldn't throw it at my financials… even read-only… we
just don't know what it's gonna do with that information yet"). Off-by-default means you opt in
on purpose, not by accident.

**When you do turn it on.** It comes bundled with **"trust but verify":** a read-only reviewer
double-checks every number *before* you approve anything, and a human still signs off. Read
access is safer than write access — start there.

---

## The mental model

Think of your agent like a **capable new hire on their first week**: give them exactly the
access the task needs, have them show you their work before it goes out, keep them away from the
company checkbook until you trust them, and never let them memorize the passwords. Guardrails
aren't training wheels you'll outgrow — they're how you scale trust safely.
