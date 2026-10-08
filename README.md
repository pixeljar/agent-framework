# Agent Starter Framework

A starter kit for building your own Claude agent — **in plain English**. You describe the
job you want handed off; Claude asks you a short set of questions and builds the technical
pieces for you. No coding required to get going.

> Built for the **Meet Claude** workshop. The through-line of the whole workshop applies here
> too: **You direct Claude. You stay in the loop.**

---

## What you'll end up with

A small, safe, reusable Claude agent that:

- does one repetitive job you describe (month-end close, a candidate shortlist, a "what am I
  waiting on" email, analyzing Slack for red flags — whatever *you* need),
- connects to the apps you already use (Google, Microsoft, Slack, QuickBooks, LinkedIn, …),
- **drafts, but never sends or changes anything without your OK** — by default,
- can later be put "on a schedule" so it runs while your laptop is closed.

---

## 60-second quickstart

1. **Copy this whole folder** into your own project folder, then open Claude Code **in that
   copy**. (Copy first, *then* open — that's how Claude sees the built-in guide.)
2. In Claude Code, type:

   ```
   /build-agent
   ```

   …or just say: **"help me build my agent."**
3. Answer the questions in plain English. Claude scaffolds your agent as you go.
4. Test it (Claude will show you how). Confirm it **drafts and waits for your approval**.
5. When you're happy, ask Claude to help you **put it on a schedule** (see `docs/05-going-cloud.md`).

That's it. Everything below is optional reading.

---

## Using a connector that needs a token

Most connectors (Google, Microsoft, Slack) sign in with a browser pop-up — nothing to set up.
A few use a token or API key instead. For those:

1. Copy `env.example` to a new file named `.env` and paste your real values there. `.env` is
   never committed or shared.
2. **Claude Code does not read `.env` on its own**, so load it into your terminal each time,
   right before you start Claude Code:

   **Mac / Linux:**
   ```
   set -a; source .env; set +a
   claude
   ```

   **Windows (PowerShell):**
   ```
   Get-Content .env | ForEach-Object { if ($_ -match '^\s*([^#=]+?)\s*=(.*)$') { Set-Item "env:$($matches[1])" $matches[2] } }
   claude
   ```
3. To confirm it worked, run `claude mcp list`. A connector that is missing its token shows a
   warning about a missing variable.

---

## What's in this folder

```
README.md            ← you are here
CLAUDE.md            ← the agent's always-on rules and guardrails
env.example          ← where your app passwords/keys go (copy to .env — never committed)
.mcp.json            ← the list of apps your agent connects to (filled in during /build-agent)
.claude/             ← the machine-readable config Claude Code reads automatically
  settings.json      ← what the agent must never do (everything else asks you first)
  skills/            ← reusable "playbooks" (incl. the /build-agent guide itself)
  agents/            ← read-only helpers: a "second set of eyes" reviewer, and the
                       eval runner /check-it uses
docs/                ← short, plain-English reference guides:
  01-scoping-worksheet.md      ← think through your agent before you build
  02-connectors-catalog.md     ← how to connect Google / Microsoft / a browser
  03-guardrails.md             ← the five safety defaults, explained
  04-model-and-cost-matrix.md  ← which Claude model for which job + spend caps
  05-going-cloud.md            ← get it off your laptop (schedules & cloud agents)
  06-when-the-connector-doesnt-exist.md ← no connector? browser path, thin wrapper, or a boundary
  07-checking-it-stays-right.md ← is it still right after a change or a model swap?
```

---

## The one rule to remember

If you only remember one thing: **this agent drafts, it doesn't act.** Anything that leaves
your hands — an email sent, a record changed, a message posted, a file deleted — waits for you
to say "approve." You can loosen that later, deliberately, once you trust it. See
`docs/03-guardrails.md`.

Not sure what to build? Open `docs/01-scoping-worksheet.md` and fill in the blanks first.
