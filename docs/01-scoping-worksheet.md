# 01 · Scoping worksheet

Fill this in before you build — on paper, in your head, or right here. The blanks line up
one-to-one with the eight questions `/build-agent` will ask, so doing this first makes the
interview fast. There are no wrong answers; a fuzzy answer is fine to start.

> Rule of thumb: a good first agent does **one** repetitive job you already know how to do
> by hand. Resist the urge to build the "everything" agent on day one.

---

**1. The job, in one sentence.**
The repetitive thing I want to hand off is:
`__________________________________________________________`

**2. How I do it today, step by step.**
(Even rough bullets help — this becomes the agent's playbook.)
- `__________________________________________`
- `__________________________________________`
- `__________________________________________`

**3. The apps it uses — and whether it just looks, or also sends/changes things.**
`Google Sheets? Gmail? Slack? Monday? QuickBooks? Outlook? Excel? Dynamics? LinkedIn Recruiter? Crelate? _______`

| App | Just look | Also send / change |
|-----|-----------|--------------------|
| `__________` | ☐ | ☐ |
| `__________` | ☐ | ☐ |

(Anything it sends or changes will wait for your approval first — see doc 03. If you list a
financial app like QuickBooks, Claude will ask whether to turn financial access on; it's off by
default, and if you turn it on a read-only reviewer double-checks every number.)

**4. What it hands back.**
The finished output is: `a draft email / a file or sheet / a summary / a shortlist / _______`
…delivered to: `__________`

**5. What makes a result good — or wrong?**
A good result looks like: `__________________________________________`
A result is wrong if: `__________________________________________`
(For a judgment job — red flags, reconciliation, candidate scoring — be specific; vague here means
vague output. These become the "known-good" examples the check-it skill uses — see doc 07.)

**6. What it must NEVER do.**
`touch money / see SSNs or PII / delete anything / post publicly / contact clients directly / _______`

**7. When should it run?**
`only when I ask`  /  `on a schedule (e.g. every morning)`  /  `when something happens`
If it runs on its own, where should it leave drafts for you? `Gmail drafts / a Google Doc / a sheet / _______`
(Schedules & cloud → doc 05.)

**8. Who signs off?**
The person who gives the OK before anything leaves your hands: `__________`

---

**You don't need to decide these** — `/build-agent` handles them and tells you what it chose:
- **Ask first or act alone?** It always asks first on a first build. You can loosen it later, one
  action at a time (doc 03).
- **Which Claude model?** It picks one that fits the job and says why. Change it any time (doc 04).

Done? Open Claude Code in this folder and type **`/build-agent`** — it'll walk the rest.
