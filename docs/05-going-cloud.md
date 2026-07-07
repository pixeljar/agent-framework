# 05 · Getting it off your laptop

Your agent works when Claude Code is open on your machine. The next step is to have it run
**when you're not there** — on a schedule, or when something happens — so you stop being the
bottleneck. This has two tiers: an easy one everyone can use, and an optional advanced one for
developers.

---

## Tier 1 — the easy path (everyone)

Good news: the config you already built is **cloud-ready as-is.** You don't rewrite anything to
move it off your laptop.

### Run it on a schedule (a "routine")

A **routine** is your agent running on a schedule in Anthropic's cloud — *laptop closed, it
still runs.* Set one up with **`/schedule`** in Claude Code, in plain English:

- "Every weekday at 7am, compile my 'what am I waiting on' email and draft it for me."
- "Every Monday, pull last week's numbers and draft the summary."

The same guardrails still apply — a scheduled agent that drafts an email still **waits for your
approval** before sending (you'll get the draft to approve). As the workshop puts it: when work
triggers *itself*, permissions matter **more**, not less.

### Always-on: a Managed Agent

For something that should run continuously or handle events, Anthropic's **Managed Agents** host
your agent in the cloud full-time. It uses the *same* pieces you already built — your
`CLAUDE.md` becomes its instructions, your `skills/` and `.mcp.json` come along unchanged. No
refactor.

### Two things to know before you schedule anything

1. **Cloud runs are billed separately.** Running in the cloud uses tokens that may be billed
   apart from your normal plan. **Set a daily/monthly spend cap** so a scheduled job can't run
   up a surprise. (See doc 04.)
2. **Guardrails matter more, not less, unattended.** Keep the approval gate on for anything
   outward. A good pattern: the agent does the work overnight and leaves you a **draft to
   approve in the morning**, rather than sending on its own.

**You do not need anything below this line to run locally or on a schedule.** Read on only if
you want to deploy the *same* agent to your own cloud (AWS, Cloudflare) unchanged.

---

## Tier 2 — portable / host-portable (advanced, optional)

*For the developers in the room.* If you want the same agent to run **unchanged** across
Anthropic's Managed Agents, Cloudflare, or AWS, you can reorganize the project so nothing in its
"substance" is tied to a specific host. This is a **re-organization, not a rewrite** — because
the four portable pieces you already built map one-to-one onto a host-neutral structure:

| Portable piece (host-neutral "core") | What you already have |
|--------------------------------------|-----------------------|
| System prompt + subagents | `CLAUDE.md` + `.claude/agents/*` |
| Skills (reusable playbooks) | `.claude/skills/*` |
| Connectors (MCP server list, no secrets) | `.mcp.json` |
| The tool-use loop | provided by every host — nothing to write |

The four things that **don't** travel are wrapped behind thin "adapters" that swap per host:

- **Credentials** — local `.env` vs. a cloud credential vault.
- **State / memory** — local files vs. a cloud memory store.
- **Triggers / schedule** — a local cron / `/schedule` routine vs. a cloud deployment schedule.
- **Approval** — the local "type approve" prompt vs. the cloud's `always_ask` tool-confirmation.

The agent's actual behavior never changes across hosts; only the adapter behind each of those
four concerns does. A fuller blueprint of this pattern (core / adapters / runners, and how to
validate portability against Anthropic's Managed Agents first, then map to Cloudflare/AWS) was
written up for the workshop — ask Claude to walk you through adapting it to your agent if you
want to go this route.

The honest guidance: **don't reach for Tier 2 unless you specifically need multi-host
portability.** For almost everyone, Tier 1 (`/schedule` → Managed Agents) is the whole answer.
