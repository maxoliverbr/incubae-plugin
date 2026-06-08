---
name: incapply
description: "Draft a program application and access outreach from an incmatch report. Usage: /incapply <incmatch-report.md>. Reads STARTUP_PROFILE.md and the incmatch report, then produces three things: tuned answers to the program's core application questions, a warm referral request to an alum/mentor, and a direct note to the program lead. Saves to incapply-<program>.md."
license: MIT
allowed-tools: Read Write
metadata:
  author: 3Flux
  version: "1.0"
  workflow-step: "6 — run after /incmatch to draft the application"
---

# /incapply — Application & Access Drafter

Turn an incmatch report into a ready-to-submit application and a warm path in. Three variants: tuned application answers, a referral request, and a direct note to the program lead.

## Invocation

```
/incapply <incmatch-report.md>
```

Example: `/incapply incmatch-techstars.md`

## Execution Steps

### Step 1 — Load Context

Read two files:

1. `STARTUP_PROFILE.md` — extract: company name, one-liner, founder names, the gap the program closes, traction proof points, website, and any alum/mentor referral contact (name + relationship).
2. The incmatch report passed as argument — extract:
   - Program name and type
   - Fit score and Recommended Action (Apply / Strengthen First / Skip)
   - What the program selects for ("The Access Path" section)
   - The "Application Questions to Prepare" list
   - The single strongest reason this program closes the startup's gap ("Why It Works")
   - Any named alum, mentor, or referral path

Derive program slug and output filename: `incmatch-techstars.md` → program = `techstars`, output = `incapply-techstars.md`.

If the Recommended Action is **Skip**, stop and tell the user: "The incmatch recommends Skip for this program. An application isn't worth the time — run `/incmatch` on a better-fit program, or revisit `/incstrat`."

If the Recommended Action is **Strengthen First**, warn at the top of the output: "⚠️ incmatch says Strengthen First — the gap is [dimension]. Consider closing it before applying; a stronger application beats an early one."

If either file is missing, stop and tell the user which one to provide.

### Step 2 — Draft Three Variants

Every variant must lead with the program-specific reason from the incmatch "Why It Works" — never a generic "we'd love to join your program."

---

#### Variant A — Application Answers

Draft answers to the program's core application questions (use the incmatch "Application Questions to Prepare" list; if the program's real questions are known, use those instead).

Rules:
- Answer each question in the program's own selection language — map each answer to what the program selects for
- Lead every answer with the specific, verifiable proof point, not the aspiration
- Be concrete with numbers from the profile — no "significant traction"
- Keep each answer tight: 60–120 words unless the question demands more
- For the "why this program" question, name the *specific* gap this program closes for this startup — the same one from the incmatch report

Format:
```
### [Application Question 1]
[Answer — 60–120 words, proof-point first]

### [Application Question 2]
[Answer]

[…continue for each question]
```

---

#### Variant B — Warm Referral Request

A note FROM the founder TO an alum or mentor (from the profile or incmatch), asking them to refer or nominate the startup. The referrer should be able to forward the second paragraph with minimal editing.

Rules:
- First paragraph (to the referrer): 2 sentences — the ask and why this program specifically
- Second paragraph (forwardable): 3–4 sentences — the gap-closing hook, the strongest proof point, the ask
- Close: thank them, offer to send more context

Format:
```
[Referrer first name],

[Ask for the referral / nomination — 2 sentences]

Here's a note you can forward directly:

---
[Program lead / selection committee],

[Forwardable note — 3–4 sentences using the incmatch hook]

[Founder Name]
[Title], [Company]
[Website]
---

Thanks [Referrer first name] — happy to send the full brief if useful.

[Founder Name]
```

---

#### Variant C — Direct Note to the Program Lead

A short email or LinkedIn message to the managing director / program lead. 3–4 sentences.

Rules:
- Sentence 1: program-specific hook — reference a graduate, a thesis, or something the program is known for
- Sentence 2: what the startup does + the single strongest proof point
- Sentence 3: the specific gap you're looking to close and why this program fits
- Sentence 4: a low-friction ask (a question about the upcoming cohort, or whether it's a fit) — not "please accept us"

Format:
```
[Body — 3–4 sentences]

[Founder Name], [Title], [Company] · [Website]
```

---

### Step 3 — Save and Report

Write all three variants to `incapply-<program>.md`.

Then tell the user:
- "Use **Variant B** if you have any alum or mentor path — a referral is the single biggest lever on acceptance."
- "Submit **Variant A** as your application, tuned to the real form once you open it."
- "Use **Variant C** only to open a relationship before the deadline — never as a substitute for the application."
- "Run `/incprep incmatch-<program>.md` before any interview so you're ready if they advance you."

---

## Output Rules

- The opening hook must come from the incmatch "Why It Works" — do not invent a new one
- Every variant must name the program and, where known, the program lead — never "Dear Program"
- Application answers must be proof-point-first and use real numbers from the profile
- The "why this program" answer must name the specific gap, matching the incmatch report — not flattery
- Tone: founder-to-peer, confident, specific. Not supplicant.
- If fit score is below 60, add a top warning: "⚠️ Fit score is [X]/100 — review the incmatch risks before spending application time."
