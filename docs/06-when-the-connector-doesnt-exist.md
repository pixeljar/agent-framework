# 06 · When the connector doesn't exist

Not every app you use has a ready-made connector — and that's fine. There's almost always a path
to reach it anyway. The trick is knowing *which* path, so you pick the right one instead of
getting stuck. This page is the decision, then a short walkthrough for each path.

> **You direct Claude. You stay in the loop.** The further you reach into an app that has no
> real connector, the more that matters — the fallback paths below are more fragile than a real
> connector, so keep them **read-only** and **approval-gated** until you trust them.

---

## First, check what's actually there

Before you decide anything, run **`/mcp`** in Claude Code. It lists the connectors actually
installed right now. Don't assume an app is connected because it "should" be — availability
changes, and half the apps people ask for (Monday.com, Crelate, QuickBooks) often aren't there
out of the box. Look first, then pick a path.

## Pick your path

Work down this list and stop at the first "yes":

| If… | Take | 
|-----|------|
| The app **shows up in `/mcp`** | **Path 1 — the ready-made connector.** Done. |
| The job is **placing a phone call** (dialing someone) | **Path 4 — the boundary.** Claude does the thinking around the call, not the call. |
| The app **has an API** but isn't in `/mcp` | **Path 3 — a thin wrapper.** A small custom connector, built for you. |
| The app has **no connector** or **blocks automation** (LinkedIn Recruiter) | **Path 2 — the browser path.** Claude drives your logged-in tab. |

## Path 1 — a ready-made connector

The easy case. If the app is in `/mcp`, sign in through its pop-up and you're done — Claude can
read and (if you allow it) act in that app. One reminder from doc 02: every connector is either
**read** or **write**. Reading is fine to leave on; anything that writes, sends, or posts stays
**approval-gated** — it drafts and waits for your OK.

## Path 2 — the browser path

For apps with **no connector**, or that **block automation** — **LinkedIn Recruiter** is the
classic case. Claude drives a **logged-in Chrome tab**: it sees and clicks the same screens you
do, in your own signed-in session. Doc 02 covers the Chrome-extension mechanics; here's the
actual sequence for the marquee example — sourcing a candidate shortlist:

1. **You log in yourself** in Chrome, and run the search you'd normally run in Recruiter.
2. The first time, you **grant per-site permission** in the Claude Chrome extension.
3. Claude **reads the profiles** on the search you point it at — it works inside *your* session.
4. It **scores each one** against the criteria you wrote down (years, skills, location, whatever).
5. It **drops a shortlist** — names, scores, and profile links — into a Google Sheet.
6. A **human reviews** the shortlist before anyone is contacted. Claude never messages a candidate.

Two honest caveats: the browser path is **slower and more fragile** than a real connector, and
some sites' terms discourage automation — go gently. Keep it **read/score-only**, keep a human
watching, and keep it approval-gated. It's for *finding and ranking*, not for reaching out.

## Path 3 — a thin wrapper

When an app **has an API** but no ready-made connector — **Crelate** and **QuickBooks Online** are
typical — Claude can build a **thin wrapper**: a small custom connector that speaks to that app's
API. This is the most technical path, but *you* don't write it — you direct, Claude builds. In
plain English:

1. **Ask Claude:** "build me a thin **read-only** connector for `<app>`'s API."
2. **Gather what it needs:** a link to the app's API docs, and an API key from the app's settings.
3. **Put the key in `.env`** (never committed) — it's referenced by name as `${VAR}`, so the agent
   uses it to sign in without ever *seeing* it. Claude Code doesn't read `.env` on its own: load
   it before you start Claude Code (see "Using a connector that needs a token" in the README).
4. **Start read-only.** Any write/send stays approval-gated, exactly like every other connector.
5. **Verify with `/mcp`** afterward — confirm the new connector shows up before you rely on it.

> **QuickBooks is financial.** Financial access is **off by default** for a reason — see doc 03.
> Turn it on deliberately during `/build-agent`, start read-only, and let the reviewer subagent
> double-check every number ("trust but verify") before you approve anything.

## Path 4 — when there's no path: phone calls

Some jobs have **no connector because they aren't a Claude tool at all.** The clearest one is
**placing outbound calls** — the *dialing* happens in a separate phone service (Twilio, Retell,
VAPI), not in Claude. Doc 02 states the boundary; here's how the agent still earns its keep around
it, for an outbound-sales job:

- The agent **scores and qualifies** your leads, and decides who's worth a call.
- It **drafts the talking points** for each qualified lead.
- It **hands that qualified list to your dialer** — the separate service that actually calls.
- After the calls, it **analyzes the results** and updates your scoring.

Everything *around* the call is fair game. The call itself isn't a Claude tool — don't expect the
agent to place it, and don't let anyone sell you otherwise.

## The mental model

There's almost always a path to an app — the question is just which one. Prefer the **safest path
that does the job**: a real connector over the browser, reading over writing, a human review over
a free hand. The more an app lacks a real connector, the more you lean on the browser path or a
wrapper — and the more staying in the loop matters, not less. When in doubt, keep it
read-only and approval-gated, and loosen one thing at a time once you've watched it work.
