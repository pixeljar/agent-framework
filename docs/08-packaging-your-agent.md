# 08 · Taking your agent with you

Your agent lives in this folder. To use it somewhere else — on your team's computers, in the
Claude app on your phone — you **package** it. In Claude Code, just type **`/package-agent`**.
It asks where the agent is going, builds the files in a `dist/` folder, and stops. It never
publishes or uploads anything: you decide where the package goes.

> **You direct Claude. You stay in the loop.** A package carries your agent's instructions and
> guardrails, but each destination enforces them a little differently. Every package comes with
> a plain-English "What's different here" note, so you know exactly what's protected.

---

## Pick where it's going

| You want to… | Choose | What you get |
|---|---|---|
| Use it in **Claude Code or Cowork**, or share it with your team | **Plugin** | A folder you (or your team) install with two commands, and update when you repackage. |
| Use it in the **Claude app** — claude.ai, desktop, or mobile | **App skill** | A zip file you upload in the Claude app. |
| Have it run **in the cloud on a schedule** | Nothing to package | Push this folder to a private GitHub repo and attach it to a routine — see doc 05. |

## What travels, and what doesn't

| | Plugin | App skill |
|---|---|---|
| The agent's job, steps, and "what good looks like" | ✅ | ✅ |
| The five guardrails | ✅ as instructions | ✅ as instructions |
| Blocked send/post/delete actions | ✅ **enforced** (a safety hook) | ⚠️ instructions only — the app's own confirmations still apply |
| `.env` files kept out of reach | ✅ **enforced** (a safety hook) | n/a — the app can't see your files |
| The read-only reviewer (financial "trust but verify") | ✅ | ⚠️ the agent re-checks numbers itself; you still sign off |
| Your apps (connectors) | ✅ the list comes along; you sign in once | ⚠️ you turn each one on in the Claude app |
| Passwords and tokens | ❌ **never** — only their names | ❌ **never** |

## Installing a plugin

1. Run `/package-agent` and choose **plugin**. It builds `dist/<your-agent>/plugin/`.
2. Just for you, on this computer: `claude plugin marketplace add ./dist/<your-agent>/plugin`, then
   `claude plugin install <your-agent>@<your-agent>`.
3. For your team: put that `plugin` folder in its own **private** GitHub repo. Teammates run
   `claude plugin marketplace add <owner>/<repo>` and the same install command. In Cowork, add the
   repo under **Customize → Plugins → Add marketplace**.
4. If your agent uses a connector with a token, each person loads their own `.env` before
   starting Claude Code (see the README). Tokens are never inside the package.

## Uploading an app skill

1. Run `/package-agent` and choose **app skill**. It builds `dist/<your-agent>/app-skill/`,
   including a `.zip`.
2. Upload the zip in the Claude app, then turn it on under **Customize → Skills**.
3. Turn on each connector the package's README lists.
4. Try it: ask for the job, and check that it drafts and waits for your OK before anything goes out.

## When you change the agent

Packages are a snapshot. After you change your agent (and `/check-it` says it still holds up),
run `/package-agent` again: the plugin's version number goes up so installs pick up the change,
and you re-upload the app skill.
