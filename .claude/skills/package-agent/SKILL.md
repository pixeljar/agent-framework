---
name: package-agent
description: >-
  Package a built agent so it can be installed elsewhere — as a Claude Code / Cowork plugin, or
  as a skill uploaded to the Claude apps (claude.ai, desktop, mobile). Builds the files in
  dist/<agent-name>/ with the guardrails included and no secrets, explains what changes in each
  destination, and stops: it never publishes, pushes, or uploads anything. Use when the user
  wants to share, install, export, package, or "take their agent with them."
---

# Package agent — take the agent somewhere else

You are turning the agent built in this folder into files another Claude surface can install.
The user is usually **not a developer**: explain each step in one plain-English line, and never
publish anything yourself — packaging is local; sharing is the user's call.

## Step 0 — check there's an agent to package

Read `AGENT.md`. If it still says `agent-status: not-built`, stop and offer `/build-agent`.
Otherwise note the agent's name, its `## Connectors it uses` table, whether financial access is
on, and whether it runs on a schedule (`## Running it`).

## Step 1 — ask where it's going

> "Where do you want to use this agent? **(a)** in Claude Code or Cowork, for you or your team
> (a plugin), **(b)** in the Claude app — claude.ai, desktop, or mobile (an uploaded skill),
> or **(c)** both?"

If they want it to run **in the cloud on a schedule**, no package is needed: point them to
`docs/05-going-cloud.md` (push this folder to a private GitHub repo and attach it to a routine).

## Step 2 — gather the pieces (read only)

Use a short, kebab-case **package name** from the agent's name (e.g. `waiting-on-digest`; at
most 64 characters, lowercase letters, digits, and hyphens only). Collect:

- **Instructions:** all of `AGENT.md`.
- **Guardrails:** all of `.claude/guardrails.md`, plus the `draft-and-approve` skill.
- **Playbook skill:** any `.claude/skills/<job>/` that `AGENT.md` points to (never a framework
  skill such as `build-agent` or `check-it`).
- **Connectors:** the agent's entries in `.mcp.json` (they contain `${VAR}` names only).
- **Blocked actions:** every `deny` rule in `.claude/settings.json` that names a connector
  action (`mcp__<server>__<action>`). Keep just the `<action>` part of each (`send_message`).
- **Reviewer:** `.claude/agents/reviewer.md`, if financial access is on or `AGENT.md` mentions
  the reviewer.

Never read or copy `.env`, `.env.*`, or `secrets/`. Never include real token values.

## Step 3a — build the plugin (Claude Code / Cowork)

Write `dist/<package-name>/plugin/` with this layout:

```
dist/<package-name>/plugin/
  .claude-plugin/
    plugin.json          ← {"name": "<package-name>", "version": "<x.y.z>",
                            "description": "<one line>", "author": {"name": "<user>"}}
    marketplace.json     ← makes this folder installable on its own (see below)
  skills/
    <package-name>/SKILL.md   ← the agent: instructions + guardrails (see below)
    draft-and-approve/SKILL.md ← copied unchanged
    <job>/SKILL.md            ← the playbook skill, if there is one, copied unchanged
  agents/reviewer.md     ← only if collected in Step 2
  .mcp.json              ← the agent's connector entries, copied unchanged
  hooks/hooks.json       ← the safety hooks (see below)
  README.md              ← install steps + "What's different here" (Step 4)
```

- **Version:** `0.1.0` the first time. If `dist/<package-name>/plugin/.claude-plugin/plugin.json`
  already exists, bump the last number (`0.1.0` → `0.1.1`) so installs pick up the change.
- **`marketplace.json`:**
  `{"name": "<package-name>", "description": "<one line>", "owner": {"name": "<user>"}, "plugins": [{"name": "<package-name>", "source": ".", "description": "<one line>"}]}`
- **The agent skill** (`skills/<package-name>/SKILL.md`): plugins don't load a `CLAUDE.md`, so the
  agent's instructions and the guardrails must live in a skill. Frontmatter: `name:
  <package-name>` and a `description` that says when to use it ("Use when the user asks to
  <the job> …"). Body: the full guardrails first, then the full `AGENT.md` (minus any `<!-- -->`
  comments). Replace mentions of `.claude/settings.json` with "this plugin's safety hooks".
- **`hooks/hooks.json`** — plugins can't ship permission rules, so the blocks become hooks. Each
  hook prints a "deny" decision and exits 0, which works in macOS/Linux shells and PowerShell:

  ```json
  {
    "hooks": {
      "PreToolUse": [
        {
          "matcher": "^mcp__.*__(<action>|<action>)$",
          "hooks": [{ "type": "command", "command": "echo '{\"hookSpecificOutput\":{\"hookEventName\":\"PreToolUse\",\"permissionDecision\":\"deny\",\"permissionDecisionReason\":\"Blocked by <package-name>: this agent saves drafts and never sends, posts, or deletes.\"}}'" }]
        },
        {
          "matcher": "Read",
          "hooks": [{ "type": "command", "if": "Read(**/.env*)", "command": "echo '{\"hookSpecificOutput\":{\"hookEventName\":\"PreToolUse\",\"permissionDecision\":\"deny\",\"permissionDecisionReason\":\"Blocked by <package-name>: .env files hold secrets.\"}}'" }]
        }
      ]
    }
  }
  ```

  Include the first entry only if Step 2 found blocked actions; list each `<action>` exactly.
  The matcher deliberately accepts any server prefix, because tool names change once
  connectors come from a plugin (`mcp__plugin_<plugin>_<server>__<action>`) or from the user's
  own Claude account. Always include the second entry.

## Step 3b — build the app skill (claude.ai, desktop, mobile)

Write `dist/<package-name>/app-skill/<package-name>/` and zip that folder:

```
dist/<package-name>/app-skill/
  <package-name>/
    SKILL.md        ← frontmatter: ONLY name and description (other keys fail the upload)
    playbook.md     ← the playbook skill's body, if there is one; SKILL.md links to it
  <package-name>.zip  ← the <package-name>/ folder at the top of the zip
  README.md         ← upload steps + "What's different here" (Step 4)
```

- **`name`:** the package name (max 64 characters). **`description`:** max **200 characters**,
  starting with when to use it ("Use when I ask for my weekly waiting-on digest…").
- **Body:** the full guardrails, then `AGENT.md`, then the `draft-and-approve` handshake
  written out inline (the app won't have the framework's other skills), then a
  **"Connectors to turn on"** list naming each app from the connector table.
- **No subagents in the app:** if financial access is on, replace "route every number through
  the read-only reviewer subagent" with "before asking for approval, re-check every number
  against its source and show the working; the human signs off."
- **Zip it:** try `zip -r <package-name>.zip <package-name>` from inside `app-skill/`; if `zip`
  isn't available, try `python3 -m zipfile -c <package-name>.zip <package-name>`. If neither
  works, tell the user how: on a Mac, right-click the folder → **Compress**; on Windows,
  right-click → **Send to → Compressed (zipped) folder**.

## Step 4 — write "What's different here"

Each package's `README.md` opens with install steps, then a plain-English **"What's different
here"** table: for each guardrail, is it **enforced** (a hook or the app blocks it) or
**instructions only** (the agent is told, nothing stops it)?

- **Plugin:** install with `claude plugin marketplace add <path-or-owner/repo>` then
  `claude plugin install <package-name>@<package-name>` (in a session:
  `/plugin marketplace add …`, `/plugin install …`). In Cowork: Customize → Plugins → Add
  marketplace, with the GitHub repo.
  Send actions and `.env` reads are **enforced** by hooks; other guardrails are instructions.
  Any `${VAR}` tokens must be loaded the same way as here (README of this framework).
- **App skill:** upload the zip in the Claude app, then turn it on under **Customize → Skills**,
  and turn on each connector in the list. Nothing is enforced by hooks in the app: the
  guardrails are instructions, backed by the app's own confirmations. Say this plainly.

## Step 5 — check, then hand over (don't publish)

1. **No secrets:** search `dist/<package-name>/` for `.env` files and for anything that looks
   like a real token (`xoxb-`, `xoxp-`, `sk-`, `ghp_`, `Bearer ` not followed by `${`). There
   must be none. Every connector credential must be a `${VAR}` name.
2. **Guardrails are in:** each agent skill contains all five guardrails.
3. **Files parse:** every `.json` file in the package parses (`python3 -m json.tool <file>`).
   If the `claude` command is available, also run `claude plugin validate` on both
   `dist/<package-name>/plugin` (checks the marketplace file) and
   `dist/<package-name>/plugin/.claude-plugin/plugin.json` (checks the plugin itself).
4. **Limits:** the app skill's `name` ≤ 64 characters, `description` ≤ 200, and its frontmatter
   has no keys besides `name` and `description`.
5. **Show the user** what was built (one line per file) and the "What's different here" table.

Then stop. Publishing is outward: if the user wants to put the plugin on GitHub or upload the
skill, show them the steps from the package `README.md` — or, if they ask you to push it for
them, use the `draft-and-approve` handshake first.
