# 02 · Connectors catalog

A **connector** is how your agent reaches an app you already use. In the workshop's words,
connectors are how "Claude reads and acts in the apps you already use" (the underlying standard
is called **MCP** — you don't need to know the acronym to use it).

Three ways to connect, from easiest to most involved:

1. **A ready-made connector** — you sign in through a pop-up; Claude can then read/act in that app.
2. **A browser path** — Claude drives a logged-in browser tab (for apps with no connector, or
   that block automation). LinkedIn Recruiter is the classic case.
3. **A thin wrapper** — for an app with an API but no ready-made connector, a small custom
   connector can be built. Ask Claude; this is the most technical option.

> **Two habits that keep you safe:**
> **(a)** Every connector is tagged below **read** or **write** — writing/sending is always
> approval-gated (doc 03). **(b)** Real passwords/keys never go in a shared file; they live in
> your local `.env`. Sign-ins are usually a browser pop-up, not a pasted token.
>
> **Always confirm a connector actually exists** by running `/mcp` in Claude Code before you
> rely on it. Don't assume — availability changes.

---

## Google stack

| App | Typical use | Read / write |
|-----|-------------|--------------|
| Google Sheets | Drop a shortlist or report into a sheet; read a tracker | read + **write** |
| Gmail | Read threads; draft replies and summaries | read + **write (send)** |
| Google Drive / Docs | Pull reference docs; save outputs | read + **write** |
| Google Calendar | Read availability; propose events | read + **write** |

## Microsoft stack

| App | Typical use | Read / write |
|-----|-------------|--------------|
| Outlook | Read mail; draft replies / summary emails | read + **write (send)** |
| Excel | Read/append a tracker or model | read + **write** |
| SharePoint | Pull reference documents | read (usually) |
| Teams | Read channels; post updates | read + **write (post)** |
| Dynamics 365 (CRM) | Read leads/opportunities; update records | read + **write** |

## Cross-platform apps

| App | Typical use | Read / write |
|-----|-------------|--------------|
| Slack | Read channels for patterns/flags; post summaries | read + **write (post)** |
| Asana | Read tasks; create/update | read + **write** |
| Monday.com | Read boards/CRM; update items | read + **write** — *verify a connector exists via `/mcp`; if not, use the browser path or a thin API wrapper* |
| Crelate (recruiting CRM) | Read candidates/jobs | read — *often needs a wrapper or browser path* |

## Financial (OFF by default — see doc 03)

| App | Typical use | Read / write |
|-----|-------------|--------------|
| QuickBooks Online | Read GL / transactions; reconcile | read (+ write only if you turn financial access on) |

Financial connectors are **not** added unless you explicitly say yes during `/build-agent`, and
when they are, a read-only reviewer double-checks every number before you approve.

---

## The browser path (for LinkedIn Recruiter, and anything with no connector)

Some systems have **no connector** or actively **block automation**. **LinkedIn Recruiter** is
the marquee example. For these, Claude can drive a **logged-in Chrome tab** — it sees and clicks
the same screens you do, in your own signed-in session.

What to know:
- You stay signed in; Claude works inside *your* session in the browser.
- It needs per-site permission in the Claude Chrome extension the first time.
- It's slower and more fragile than a real connector, and some sites' terms discourage
  automation — go gently, keep a human watching, and keep it **read/score-only** with a human
  reviewing the shortlist. This is always approval-gated.

Typical shape: Claude reads candidate profiles in LinkedIn Recruiter, scores them against your
criteria, and drops a shortlist (with profile links) into a Google Sheet for a recruiter to
review — never messaging anyone without approval.

---

## One thing connectors can't do: make phone calls

If your job involves **outbound calls** (Twilio, Retell, VAPI, etc.), the *dialing* happens in a
separate phone service — it is **not** a Claude tool. Your agent can still do everything around
it: score and qualify leads, decide who's worth a call, draft the talking points, and analyze
results. Just don't expect the agent itself to place the call.
