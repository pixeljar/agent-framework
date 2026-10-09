# How this framework works

> Guiding principle: **You direct Claude. You stay in the loop.**

## What this project is

A starter template for a non-technical person to build one small, safe Claude agent by
describing the job in plain English. The primary entry point is the `build-agent` skill
(`/build-agent`), which interviews the user and scaffolds their agent. Reference material for
the human lives in `docs/`.

## Who owns which files

The framework and the agent live side by side in this folder, in separate files:

- **Framework-owned — never change these while building or running an agent:** `CLAUDE.md`,
  `.claude/framework.md`, `.claude/guardrails.md`, the `build-agent`, `check-it`,
  `draft-and-approve`, and `package-agent` skills, the `reviewer` and `eval-runner`
  subagents, `README.md`, and `docs/`. They change only when the user explicitly asks to
  change the framework itself.
- **Agent-owned — written by `/build-agent`:** `AGENT.md` (the agent's job and playbook), an
  optional `.claude/skills/<job>/` playbook skill, `state/` (anything the agent saves between
  runs on this computer; gitignored), `evals/` (written by `/check-it`),
  `archive/` (earlier agents set aside by a replace — not loaded, kept so a replace can be
  undone), and `dist/` (packages built by `/package-agent`; gitignored).
- **Shared config — `/build-agent` adds entries** (and, when replacing an agent, removes only
  that agent's entries after recording them in `archive/`): `.mcp.json` (connectors),
  `.claude/settings.json` (deny rules), and `env.example` (token names).

If `AGENT.md` still says `agent-status: not-built`, no agent has been built here yet.

## How to work with the user here

- Most users are **not developers.** Explain in plain English, choose sensible defaults, and
  ask before doing anything that would surprise them.
- This is **not** a WordPress project — WordPress coding standards do not apply. Follow the
  user's global git conventions (feature branch, imperative-present commit messages, show the
  diff before committing).

## When the user says "build my agent" (or types /build-agent)

Run the `build-agent` skill. It carries the full interview script and the templates that turn
their answers into config. Don't improvise the interview — follow the skill.
