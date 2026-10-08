# The interview script

Ask these **in order**, in plain English, one or two at a time. Offer multiple-choice options
where shown so the user can just pick. After the last one, read back a plain-English summary and
get a "yes" before writing any files.

The right-hand column is **for you** (Claude), not the user — it's where each answer lands.

| # | Ask the user (plain English) | What you do with the answer |
|---|---|---|
| 1 | "In one sentence — what repetitive job do you want to hand off?" | Name the agent + write its mission → `AGENT.md` heading + `## What this agent does` |
| 2 | "Walk me through how you do it today, step by step." | Capture the playbook → `## How it works`. If it's a clean repeatable procedure, offer to save it as its own reusable skill under `.claude/skills/<job>/SKILL.md`. |
| 3 | "Which apps does it use — and in each one, should it just **look**, or also **send or change** things?" *(menu: Google Sheets / Gmail / Google Drive · Slack · Asana · Monday.com · QuickBooks Online · Microsoft Outlook / Excel / SharePoint / Teams · Microsoft Dynamics · LinkedIn Recruiter · Crelate · other)* | Pick connectors → `.mcp.json` entries (from `templates/mcp.snippet.json.tmpl`) + the `## Connectors it uses` table, marking each read or write. Write/send → **approval-gated** (`draft-and-approve`). If an app has no API / blocks automation (LinkedIn Recruiter is the classic case), choose the **browser path** — see `docs/02-connectors-catalog.md`, step-by-step in `docs/06-when-the-connector-doesnt-exist.md`. **If they name a financial app** (QuickBooks, bank feeds, payroll, Dynamics finance), ask the financial follow-up below. |
| 4 | "What should it hand back to you?" *(menu: a draft email · a file/sheet · a summary · a shortlist · something else)* | Set the output → `## Output`. Default to **draft, not send.** |
| 5 | "What makes a result **good** — and what would make one **wrong**?" *(e.g. what counts as a 'red flag'; what a reconciliation must tie out to; what makes a candidate a strong match)* | Capture the judgment rubric → `## What "good" looks like`. A simple mechanical job needs only a one-line "good = X, wrong = Y"; a judgment-heavy job needs the specifics. This seeds the golden set — see the `check-it` skill / `docs/07-checking-it-stays-right.md`. |
| 6 | "What should it NEVER do?" *(menu: touch money / financials · see SSNs or PII · delete anything · post publicly · contact clients directly · other)* | Hard nos → add `deny` rules via `templates/settings.snippet.json.tmpl` + `## What it must NEVER do`. |
| 7 | "When should it run — only when you ask, on a schedule, or when something happens?" | On-request now. Schedule/event → ask a follow-up: *"Since no one will be there to approve, where should it leave its drafts for you — Gmail drafts, a Google Doc, a Sheet, or somewhere else?"* Note the answer in `## Running it`, block the send actions (see "Scheduled runs" in `SKILL.md`), and point to `docs/05-going-cloud.md` (offer to walk them through typing `/schedule` to set up a routine — only the user can run it). |
| 8 | "Before anything leaves your hands, it drafts and waits for an OK. Who gives that OK — you, or someone else?" | Name the human sign-off → `## Human reviewer`. Offer the read-only `reviewer` subagent as a machine pre-check. |

### The financial follow-up (only if Q3 named a financial app)

> "That's a financial system. Financial access is **off** by default — do you want to turn it on
> for this agent? If you do, a read-only reviewer double-checks every number before you approve."
> *(default: No)*

**No** → leave that app out; financial stays off. **Yes** → add the financial connector (start
read-only) AND turn on the `reviewer` subagent; write "Financial access ON — trust but verify"
into `## Guardrails`.

### What you decide without asking

- **Approval first.** Every outward action drafts and waits for the user's OK — that's the
  non-negotiable default for a first build, so don't ask whether to turn it on. Say it in the
  read-back.
- **The model.** Pick the tier from `docs/04-model-and-cost-matrix.md` based on the job (start on
  Sonnet; Haiku for high-volume shallow work; Opus for deep judgment), confirm the current model
  ID with `/claude-api`, and write it into `## Model & cost`. Say which tier and why in one line
  of the read-back, so the user can change it. Only ask if they bring up cost or quality.

## Notes for specific answers

- **Defining "good" (Q5)** — for judgment jobs (red flags, reconciliation, candidate scoring),
  push past "I'll know it when I see it" to concrete, checkable criteria; those become the golden
  set the `check-it` skill scores against. For a simple mechanical job, a one-line good/wrong is plenty.
- **"I'm not sure which apps"** — offer `docs/02-connectors-catalog.md` and let them point at the
  systems they live in day-to-day. Half the group is a Google shop, half Microsoft; support both.
- **Phone calls / outbound dialing** — the agent does the scoring/qualification/analysis; the
  actual calling is a separate service (Twilio/Retell/VAPI), not a Claude tool. Be honest about
  the boundary.
- **"Just do it for me, don't ask"** — you can accommodate this *later*, per-action, once they've
  seen it work. Do not disable the approval gate during the first build. Explain that.

## The read-back (do this before writing files)

> "Here's what I'll build: an agent called **<name>** that **<mission>**. It reads **<apps>** and
> can write to **<apps>** — but it will **draft and wait for your OK** before anything leaves your
> hands. A good result looks like **<rubric>**. It will never **<hard nos>**. Financial access is
> **<on/off>**. It runs **<when>** *(if scheduled: "and because no one's there to approve, it
> saves drafts to **<destination>** and can't send anything")*, and **<person>** reviews the
> output. I'll run it on **<tier>** because **<one-line why>**. Did I get that right?"

Only after "yes" → scaffold with the templates.
