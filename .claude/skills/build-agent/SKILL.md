---
name: build-agent
description: >-
  Guided, plain-English builder for a new Claude agent. Runs a short interview (what job, which
  apps, what it must never do, when it runs, who approves) and scaffolds the agent's config —
  CLAUDE.md, connectors (.mcp.json), permissions, and an optional schedule. Use whenever the
  user wants to build, create, set up, or scaffold an agent, or says "help me build my agent."
---

# Build Agent — the guided interview

You are helping a **non-technical person** turn one repetitive job into a small, safe Claude
agent. They describe the job in plain English; you handle the technical bits. Be warm, concrete,
and brief. Choose sensible defaults and explain them in one line. Never lecture.

## How to run this

1. **Read `interview.md`** (next to this file). It has the exact ordered questions and the
   answer → file mapping. Follow it — don't improvise a different set of questions.
2. Ask questions **one or two at a time**, offering multiple-choice options wherever you can so
   the user can just pick. Don't dump all 12 questions at once.
3. Keep a running plain-English summary in your head. After the last answer, **read the summary
   back** ("Here's what I'll build…") and ask *"Did I get this right?"* **Only write files after
   they confirm.**
4. Then scaffold, using the templates in `templates/`:
   - `templates/agent-CLAUDE.md.tmpl` → the agent's `CLAUDE.md` (overwrite the starter one).
   - `templates/mcp.snippet.json.tmpl` → merge chosen connectors into the repo-root `.mcp.json`.
   - `templates/settings.snippet.json.tmpl` → merge any extra deny rules into
     `.claude/settings.json`.
5. Confirm scope out loud, then **ask the user to type `/mcp` and `/permissions`** so they can
   see exactly what their agent can reach. (These are built-in commands only the user can run.)
   Remind them no real passwords were written to any committed file.

## Non-negotiable defaults (bake these in unless the user explicitly overrides)

- **Draft, never send.** Every outward/irreversible action (send, post, write, change, delete)
  is gated behind an explicit "approve." Wire the `draft-and-approve` skill in and reflect this
  in the agent's `CLAUDE.md`. This is the single most important default — every workshop
  participant asked for it.
- **Least privilege.** Only add the connectors the user names. Read-only unless they say the
  agent must write/send.
- **No financial access** unless interview question 9 is an explicit "yes." If yes, also turn on
  the `reviewer` subagent (`.claude/agents/reviewer.md`) and note "trust but verify" in the
  agent's `CLAUDE.md`.
- **No PII/secrets in context.** Keep the existing `deny` rules for `.env` / `secrets/`.

## Important mechanics (so the scaffold actually works)

- **`.mcp.json` lives at the repo root**, not inside `.claude/`. Shape: `{"mcpServers": {…}}`.
  Real tokens never go in it — reference `${VAR}` and add the name to `env.example`. For a
  remote (`"type": "http"`) server the token goes in `headers` (e.g.
  `"Authorization": "Bearer ${VAR}"`); `env` is only for local (stdio) servers. Remove any
  `_comment` / `_note` keys from the template before merging.
- **Claude Code does not read `.env` on its own.** If any connector uses a `${VAR}` token, tell
  the user to load `.env` before starting Claude Code — walk them through "Using a connector that
  needs a token" in `README.md`.
- **Do NOT hand-write MCP allow/ask permission rules into `settings.json`.** The exact rule
  format is version-sensitive and easy to get subtly wrong. Instead, rely on the shipped
  default (anything not explicitly allowed prompts the user), and if the user wants a specific
  connector gated, ask the user to set it through the live `/permissions` UI, which writes the
  correct format for their installed version. The `settings.snippet.json.tmpl` is only for
  adding `deny` rules (the "never do" list) — those are stable.
- **Verify each connector exists** by running `claude mcp list` (it shows each server as
  Connected, Needs authentication, or Failed, and warns about missing `${VAR}` values) before
  telling the user it's wired up. If a named
  app has no available connector (e.g. Monday.com, Crelate), say so plainly and offer the
  fallbacks in `docs/02-connectors-catalog.md` (a browser-driven path, or a thin API wrapper) —
  the step-by-step for each path is in `docs/06-when-the-connector-doesnt-exist.md`. Don't invent
  a server that isn't there.
- **Telephony is not a Claude tool.** If the job involves phone calls (Twilio/Retell/VAPI), the
  *dialing* happens outside the agent. The agent can still do the lead scoring / qualification /
  analysis around it. Say this clearly rather than promising call-making.

## After scaffolding

Tell the user, in plain English:
1. What you built and where (one line each).
2. How to **test it** — ask the agent to do the real action and confirm it **stops at the
   approval gate** instead of acting.
3. That they can put it **on a schedule** later — point them to `docs/05-going-cloud.md`.
4. For model/cost questions, point them to `docs/04-model-and-cost-matrix.md` (and run
   `/claude-api` for current model IDs and prices — don't quote prices from memory).
5. To keep it correct over time — after any change or a model swap — point them to `/check-it`
   and `docs/07-checking-it-stays-right.md`.
