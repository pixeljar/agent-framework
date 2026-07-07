# The interview script

Ask these **in order**, in plain English, one or two at a time. Offer multiple-choice options
where shown so the user can just pick. After the last one, read back a plain-English summary and
get a "yes" before writing any files.

The right-hand column is **for you** (Claude), not the user — it's where each answer lands.

| # | Ask the user (plain English) | What you do with the answer |
|---|---|---|
| 1 | "In one sentence — what repetitive job do you want to hand off?" | Name the agent + write its mission → `CLAUDE.md` heading + `## What this agent does` |
| 2 | "Walk me through how you do it today, step by step." | Capture the playbook → `## How it works`. If it's a clean repeatable procedure, offer to save it as its own reusable skill under `.claude/skills/<job>/SKILL.md`. |
| 3 | "Which apps and data does it touch?" *(menu: Google Sheets / Gmail / Google Drive · Slack · Asana · Monday.com · QuickBooks Online · Microsoft Outlook / Excel / SharePoint / Teams · Microsoft Dynamics · LinkedIn Recruiter · Crelate · other)* | Pick connectors → `.mcp.json` entries (from `templates/mcp.snippet.json.tmpl`) + `## Connectors it uses`. If an app has no API / blocks automation (LinkedIn Recruiter is the classic case), choose the **browser path** — see `docs/02-connectors-catalog.md`, step-by-step in `docs/06-when-the-connector-doesnt-exist.md`. |
| 4 | "For each app — should it only READ, or also WRITE / SEND / CHANGE things?" | Mark each connector read vs write. Read → fine to allow. Write/send → **approval-gated** (see Q6). Record in the connector table. |
| 5 | "What should it produce for you?" *(menu: a draft email · a file/sheet · a summary · a shortlist · something else)* | Set the output → `## Output`. Default to **draft, not send.** |
| 6 | "Can it act on its own, or must you approve before anything leaves your hands — an email sent, a record changed, a message posted?" *(default: approve first)* | Turn on the `draft-and-approve` handshake; note it in `## Guardrails`. Leave outward tools un-allowed so they prompt. |
| 7 | "What should it NEVER do?" *(menu: touch money / financials · see SSNs or PII · delete anything · post publicly · contact clients directly · other)* | Hard nos → add `deny` rules via `templates/settings.snippet.json.tmpl` + `## What it must NEVER do`. |
| 8 | "Does it need money / financial access — e.g. QuickBooks?" *(default: No)* | **No** → financial stays off. **Yes** → add the financial connector AND turn on the `reviewer` subagent; write "Financial access ON — trust but verify" into `## Guardrails`. |
| 9 | "For this job, what matters more — speed and low cost, or the deepest possible judgment?" | Pick a model tier from `docs/04-model-and-cost-matrix.md`; confirm the current model ID/price with `/claude-api`. Note it in `## Model & cost`. |
| 10 | "When should it run — only when you ask, on a schedule, or when something happens?" | On-request now. Schedule/event → note it in `## Running it` and point to `docs/05-going-cloud.md` (offer to set up a `/schedule` routine). |
| 11 | "Who reviews its output before it counts?" | Name the human sign-off → `## Human reviewer`. Offer the read-only `reviewer` subagent as a machine pre-check. |

## Notes for specific answers

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
> hands. It will never **<hard nos>**. Financial access is **<on/off>**. It runs **<when>**, and
> **<person>** reviews the output. Did I get that right?"

Only after "yes" → scaffold with the templates.
