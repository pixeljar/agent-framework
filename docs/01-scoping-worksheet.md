# 01 · Scoping worksheet

Fill this in before you build — on paper, in your head, or right here. The blanks line up
one-to-one with the questions `/build-agent` will ask, so doing this first makes the interview
fast. There are no wrong answers; a fuzzy answer is fine to start.

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

**3. What it reads.**
Which apps / files / data does it need to look at?
`Google Sheets? Gmail? Slack? Monday? QuickBooks? Outlook? Excel? Dynamics? LinkedIn Recruiter? Crelate? _______`

**4. What it writes or sends (if anything).**
Which of those does it also need to change, send to, or post to?
`__________________________________________`
(Everything here will require your approval before it happens — see doc 03.)

**5. What it hands back.**
The finished output is: `a draft email / a file or sheet / a summary / a shortlist / _______`
…delivered to: `__________`

**6. Act on its own, or ask first?**
Default and recommended: **ask first.** Circle one: `ask first`  /  `act on its own (later)`

**7. What it must NEVER do.**
`touch money / see SSNs or PII / delete anything / post publicly / contact clients directly / _______`

**8. Does it need financial access?**
`No` (default)  /  `Yes — it must reach QuickBooks / Dynamics finance / bank data`
(If yes, a read-only reviewer will double-check every number — see doc 03.)

**9. Speed & cost, or deepest judgment?**
For this job I care more about: `speed + low cost`  /  `the best possible judgment`  /  `a balance`
(This picks the Claude model — see doc 04.)

**10. When should it run?**
`only when I ask`  /  `on a schedule (e.g. every morning)`  /  `when something happens`
(Schedules & cloud → doc 05.)

**11. Who signs off?**
The person who reviews the output before it counts: `__________`

---

Done? Open Claude Code in this folder and type **`/build-agent`** — it'll walk the rest.
