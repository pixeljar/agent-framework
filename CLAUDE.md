# CLAUDE.md — Agent rules

This file is loaded automatically every time Claude Code runs in this folder. It pulls in the
**always-on guardrails** for any agent built here (from `.claude/guardrails.md`, below). These
apply even before an agent is fully scoped — they hold during the build itself.

> Guiding principle: **You direct Claude. You stay in the loop.**

## What this project is

A starter template for a non-technical person to build one small, safe Claude agent by
describing the job in plain English. The primary entry point is the `build-agent` skill
(`/build-agent`), which interviews the user and scaffolds their agent. Reference material for
the human lives in `docs/`.

## Always-on guardrails (do not override without the user's explicit say-so)

@.claude/guardrails.md

## How to work with the user here

- Most users are **not developers.** Explain in plain English, choose sensible defaults, and
  ask before doing anything that would surprise them.
- This is **not** a WordPress project — WordPress coding standards do not apply. Follow the
  user's global git conventions (feature branch, imperative-present commit messages, show the
  diff before committing).

## When the user says "build my agent" (or types /build-agent)

Run the `build-agent` skill. It carries the full interview script and the templates that turn
their answers into config. Don't improvise the interview — follow the skill.
