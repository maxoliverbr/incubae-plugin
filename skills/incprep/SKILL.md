---
name: incprep
description: "Prepare for a program selection interview from the active startup profile and an incmatch report. Usage: /incprep <incmatch-report.md>. Produces complete interview prep: a timed agenda, answers to the program's likely selection questions, rebuttals to the incmatch risk flags, the honest 'why this program / why now' story, and the questions to ask them back. Saves to incprep-<program>.md."
license: MIT
allowed-tools: Read Write
metadata:
  author: 3Flux
  version: "1.0"
  workflow-step: "7 — run after /incmatch when a selection interview is booked"
---

# /incprep — Program Selection Interview Prep

Prepare the founders for a program selection interview (the partner/managing-director conversation that decides admission) using the startup profile and a previously generated incmatch report.

## Invocation

```
/incprep <incmatch-report.md>
```

Example: `/incprep incmatch-techstars.md`

## Execution Steps

### Step 1 — Load Context

Read two files:

1. `STARTUP_PROFILE.md` — the full profile.
2. The incmatch report — extract: program name and type, fit score and Recommended Action, what the program selects for, the "Application Questions to Prepare," the Key Risks & Costs, and the strongest gap-closing reason.

If the Recommended Action is **Skip**, stop and tell the user: "incmatch says Skip — don't spend prep time on a program you shouldn't join."

Derive slug: `incmatch-techstars.md` → output `incprep-techstars.md`.

If either file is missing, stop and tell the user which one to provide.

### Step 2 — Produce the Prep

Selection interviews are usually 20–30 minutes and are really testing three things: *are these founders coachable and high-velocity, is the gap one we can actually close, and will this team make us look good?* Prep to all three.

---

## 1. Timed Agenda (assume ~25 minutes)

| Time | Segment | Goal |
|------|---------|------|
| 0:00–2:00 | Hook + one-liner | Land the problem and why you, fast |
| 2:00–7:00 | Traction + proof | Show velocity and what's real |
| 7:00–14:00 | Their questions | Selection grilling — see Section 3 |
| 14:00–20:00 | Why this program / why now | The gap only they close — Section 4 |
| 20:00–24:00 | Your questions for them | See Section 6 — this is also a test |
| 24:00–25:00 | Clear close | Restate fit + next step |

## 2. Opening Hook (verbatim)

Write a 2–3 sentence opening the founder can say cold: the problem, the wedge, and the single most credible proof point. Pull the hook from the incmatch "Why It Works."

## 3. Likely Selection Questions — with Answers

For each question the program is likely to ask (from incmatch + the categories below), give: the question, a tight model answer (proof-first), and an "if they push" follow-up.

Cover at minimum:
- **Why you / why this team** — the founder-market-fit answer
- **What's actually working** — traction with numbers, honest about what's not
- **What do you need from us** — the *specific* gap (this is the make-or-break answer; vague = rejected)
- **What will you do in 90 days** — velocity and a concrete plan
- **Coachability** — a real example of changing course on evidence

```
### [Question]
**Answer:** [proof-first, ≤60 seconds spoken]
**If they push:** [the follow-up answer]
```

## 4. Why This Program / Why Now

Three sentences the founder must be able to deliver without notes:
1. The specific gap this program closes that the startup can't close as fast alone (from incmatch)
2. Why now — why this cohort, this stage
3. What the program gets from backing this team — make it mutual, not supplicant

Then list **two things NOT to say** — common founder mistakes that signal low fit or low velocity for this program.

## 5. Risk Rebuttals

For each Key Risk from the incmatch report, a one-line rebuttal the founder can deliver calmly if it comes up.

| Their likely concern | Your one-line rebuttal |
|----------------------|------------------------|
| [Risk 1 from incmatch] | [rebuttal] |
| [Risk 2] | [rebuttal] |

## 6. Questions to Ask Them (diligence both ways)

Selection is mutual. Five sharp questions that (a) signal seriousness and (b) surface whether the program is baloney before the founder commits. Draw from the incposer/incperks open questions if those files exist.

1. [e.g., "What did your last 3 cohorts' median outcome look like 12 months out?"]
2. [e.g., "Which mentors would actually be matched to us, and how often do they engage?"]
3. [e.g., "What customer or investor introductions do graduates in our sector typically get?"]
4. [e.g., a question on the real terms / IP]
5. [e.g., "What kind of company has this program *not* been a good fit for?"]

## 7. The Close

One sentence restating the mutual fit and a clear next step. Confident, not needy.

---

### After Saving

Save to `incprep-<program>.md`. Then tell the user, out of prep voice:
- "Rehearse Section 3 out loud. Any answer that runs past 60 seconds or gets vague on 'what do you need from us' is the one that loses you the spot."
- "Section 6 is not a formality — if their answers are weak, the program may be the baloney `/incposer` warned about. Be willing to walk."

---

## Output Rules

- Every answer must use real numbers and facts from the profile — no placeholders left unfilled
- The "what do you need from us" answer must name the specific gap from incmatch — this is the highest-stakes answer
- Section 6 questions must be answerable in a way that would actually change the founder's decision — not softballs
- Keep model answers to ≤60 seconds spoken (~120 words)
- Tone: high-velocity, coachable, confident — the three things selection committees screen for
