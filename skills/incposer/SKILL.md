---
name: incposer
description: "Detect entrepreneur-support programs that are baloney: zombie programs running on reputation, demo-day theater with no outcomes, extractive equity-for-nothing accelerators, logo-wall mentor networks, and coordination-overhead programs that consume founder time without closing a gap. Usage: /incposer <Program Name>. Runs 8 evidence-based checks, scores a Baloney Score (0–100), and recommends whether to apply. Saves to incposer-<program>.md."
license: MIT
compatibility: Requires Claude Code with WebSearch and WebFetch. Internet connection required.
allowed-tools: WebSearch WebFetch Read Write
metadata:
  author: 3Flux
  version: "1.0"
  workflow-step: "3 — run before /incmatch to verify a program is real and serves founders"
---

# /incposer — Program Baloney Detector

Run a structured 8-check screen against an entrepreneur-support program to determine whether it actually serves founders or just consumes their time and equity. Produces a **Baloney Score** and a verdict before a founder spends weeks on an application or, worse, a quarter inside a program that closes no real gap.

A program can look prestigious and still be baloney for *your* startup: a dead program coasting on a famous name, a demo-day spectacle with no real investors in the room, a mentor wall of people who never show, or an "ecosystem" of meetings and reporting that quietly eats the time you should spend building and selling.

## Invocation

```
/incposer <Program Name>
```

Examples: `/incposer Techstars` or `/incposer "gener8tor"`

## Execution Steps

### Step 1 — Load Startup Context

Read `STARTUP_PROFILE.md` from the current working directory. Extract and hold in context:

- Stage (idea / prototype / MVP / revenue)
- Sector and category
- Whether the startup is capital-intensive (deep tech/hardware) or capital-light (software)
- The specific gap a program should close (capital / customers / credibility / co-founders / forcing function)
- Geography and willingness to relocate

This context calibrates the checks. A program with no recent demo-day investors is fatal for a software team that needs capital, but irrelevant for a deep-tech team using a non-dilutive grant program purely for milestone funding. Flag relative risk, not just absolute signals.

If `STARTUP_PROFILE.md` does not exist, stop and tell the user to create it first.

Derive the program slug and output filename: lowercase the program name, strip spaces and punctuation.
- `Techstars` → `incposer-techstars.md`
- `Y Combinator` → `incposer-y-combinator.md`

### Step 2 — Gather Raw Intel

Use WebSearch and WebFetch across exactly these 6 angles. Run all 6 before moving to Step 3 — do not score until you have the full picture.

1. `"<Program Name>" cohort 2024 2025 2026 application` — is it still running batches?
2. `"<Program Name>" alumni companies raised acquired shut down` — outcome evidence
3. `"<Program Name>" equity terms investment fee how much they take` — cost/value structure
4. `"<Program Name>" mentors curriculum schedule demo day` — substance vs. theater
5. `"<Program Name>" founder review experience worth it scam OR disappointed` — founder sentiment
6. `site:<program-website> portfolio OR alumni OR companies` — actual graduate list for outcome analysis

Pull direct quotes, dates, and named graduate companies wherever possible. Named outcomes beat marketing copy.

**If fewer than 3 of the 6 angles return usable data** (program is very small, new, or opaque): note this explicitly in the report header and default the verdict to **Watch List**. Do not score what you cannot evidence.

### Step 3 — Run the 8 Baloney Checks

Run each check in sequence. For each, produce:
- **Signal:** 🟢 Green / 🟡 Yellow / 🔴 Red
- **Finding:** 1–2 sentences with specifics — dates, numbers, named companies
- **Evidence:** the source (URL, quote, or named publication)

Signal thresholds are defined per check below.

---

#### Check 1: Program Vitality (20 pts)
**What it tests:** Is it still running real cohorts, or coasting on a famous name? Active programs run batches on a predictable cadence.

- 🟢 Ran a cohort within the last 12 months and has an open or announced next batch
- 🟡 Last cohort 12–24 months ago, or cadence has visibly slowed — winding down risk
- 🔴 No cohort in 24+ months, or the program appears defunct / acquired / renamed with no activity — zombie

**Points:** 🟢 = 20 · 🟡 = 10 · 🔴 = 0

---

#### Check 2: Outcome Evidence (20 pts)
**What it tests:** Do graduates actually progress — raise follow-on, generate revenue, get acquired, survive? Or is the alumni page a graveyard of dead landing pages? This is the single most important check.

- 🟢 Multiple recent graduates (last 3 cohorts) with verifiable progress — funding, revenue, acquisition, or clear growth
- 🟡 Some success stories but mostly older, or a long tail of inactive companies — mixed track record
- 🔴 No verifiable graduate outcomes, alumni are dormant/dead, or all "wins" predate the last 3 years — outcome theater

**Points:** 🟢 = 20 · 🟡 = 10 · 🔴 = 0

---

#### Check 3: Cost Honesty (15 pts)
**What it tests:** What you give up (equity, fee, IP rights) vs. what you demonstrably get back. Extractive programs take equity or fees and deliver little beyond a certificate.

- 🟢 Terms are transparent and proportionate to value (non-dilutive, modest equity for real capital + access, or low/no fee)
- 🟡 Terms are defensible but rich for the stage (e.g., high equity with mostly-network value), or terms are not clearly published
- 🔴 Extractive or opaque: significant equity/fee with no commensurate capital or access, hidden costs, or any IP-assignment clause

**Points:** 🟢 = 15 · 🟡 = 8 · 🔴 = 0

---

#### Check 4: Curriculum Substance vs. Theater (10 pts)
**What it tests:** Is there a real operating curriculum and structured work, or is it generic content, "fireside chats," and a demo day that exists mainly for the program's own marketing?

- 🟢 Specific, operator-led curriculum with measurable founder outputs (customer dev, hiring, GTM, fundraising readiness)
- 🟡 Real content but generic or lecture-heavy — useful for first-time founders, thin for experienced ones
- 🔴 Mostly events, photo-ops, and demo-day theater; no evidence of substantive founder work product

**Points:** 🟢 = 10 · 🟡 = 5 · 🔴 = 0

---

#### Check 5: Mentor & Network Quality (10 pts)
**What it tests:** Are mentors real, relevant operators and investors who actually engage — or a logo wall of famous names who never show up? Quality and *attendance* both matter.

- 🟢 Named, relevant mentors with evidence of real, recurring engagement with founders
- 🟡 Strong names listed but unclear how engaged they are, or mentors are general rather than sector-relevant
- 🔴 Logo-wall network with no evidence of access, or mentors irrelevant to the startup's sector and stage

**Points:** 🟢 = 10 · 🟡 = 5 · 🔴 = 0

---

#### Check 6: Capital Reality (10 pts)
**What it tests:** If the program promises investment or a demo day, does the money actually move? Do real, check-writing investors attend — or is demo day a room of service providers and other founders?

- 🟢 Stated investment reliably wired, and demo day demonstrably draws active investors who have written checks to graduates
- 🟡 Capital is real but small, or demo-day investor turnout is unclear / inconsistent
- 🔴 Promised capital is conditional/illusory, or demo day has no evidence of real investor follow-through — *(score N/A and reallocate if the founder's gap is not capital)*

**Points:** 🟢 = 10 · 🟡 = 5 · 🔴 = 0

---

#### Check 7: Coordination Overhead (10 pts)
**What it tests:** The time tax. Mandatory events, relocation, standups, reporting, "ecosystem" obligations — all measured against the founder's scarcest resource. A program that fills the calendar but doesn't close a gap is a net negative even if it's free.

- 🟢 Light, flexible time commitment, or the required time directly produces founder value (customers, capital, hires)
- 🟡 Moderate mandatory load (relocation optional, several required events) — tolerable if the value is high
- 🔴 Heavy mandatory overhead (forced relocation, daily obligations, extensive reporting) with weak evidence it converts to outcomes — busywork that starves build/sell time

**Points:** 🟢 = 10 · 🟡 = 5 · 🔴 = 0

---

#### Check 8: Founder Sentiment (5 pts)
**What it tests:** What alumni actually say once the cohort ends. Search founder interviews, X/Twitter threads, Reddit/Blind/forum posts, and reviews. Patterns of ghost mentors, broken capital promises, or equity regret are disqualifying.

- 🟢 Positive alumni references, or "would do it again" sentiment with specifics
- 🟡 Mixed or thin feedback — not damning, but not a ringing endorsement
- 🔴 Documented pattern of disappointment: absent mentors, unmet promises, equity regret, or "waste of a quarter"

**Points:** 🟢 = 5 · 🟡 = 3 · 🔴 = 0

---

### Step 4 — Score and Verdict

Sum the points across all 8 checks. If Check 6 (Capital Reality) is genuinely **N/A** because the founder's gap is not capital, redistribute its 10 points proportionally across Checks 2, 4, and 7 and note this in the report.

**Baloney Score: XX/100** *(higher = more legitimate, less baloney)*

Show the per-check breakdown in a table:

| Check | Signal | Points Earned / Max |
|-------|--------|---------------------|
| 1. Program Vitality | 🟢/🟡/🔴 | X / 20 |
| 2. Outcome Evidence | 🟢/🟡/🔴 | X / 20 |
| 3. Cost Honesty | 🟢/🟡/🔴 | X / 15 |
| 4. Curriculum Substance | 🟢/🟡/🔴 | X / 10 |
| 5. Mentor & Network Quality | 🟢/🟡/🔴 | X / 10 |
| 6. Capital Reality | 🟢/🟡/🔴 | X / 10 |
| 7. Coordination Overhead | 🟢/🟡/🔴 | X / 10 |
| 8. Founder Sentiment | 🟢/🟡/🔴 | X / 5 |
| **Total** | | **XX / 100** |

**Verdict:**

| Score | Verdict | What it means |
|-------|---------|---------------|
| 80–100 | **Legit** | Real program, real outcomes, fair terms — worth a serious application |
| 60–79 | **Probable** | Yellow flags present; apply if the fit is strong, but clarify open questions first |
| 40–59 | **Watch List** | Material baloney signals; only worth it with a warm path or a unique gap-closer |
| 0–39 | **Baloney** | Don't spend the application time — the cost (equity/fee/hours) outweighs the value |

### Step 5 — Save and Report

Write the full check report to `incposer-<program>.md`.

Then tell the user:
- **Legit (80–100):** "This program checks out. Run `/incmatch <program>` to see if it closes *your* specific gap."
- **Probable (60–79):** "Yellow flags noted — run `/incmatch <program>` but weigh these signals against the gap it would close."
- **Watch List (40–59):** "De-prioritize unless you have an alum/mentor path or it's the only source of a gap-closer you can't get elsewhere."
- **Baloney (0–39):** "Skip it. Run `/inclist` to surface programs that actually move the needle — and remember `/incstrat` may say no program is the right answer."

---

## Output Format

When producing the output file, read the exact template from [references/output-format.md](references/output-format.md).

---

## Output Rules

- Every check must cite named evidence — URLs, dates, quotes, or named graduate companies. If evidence is absent, say what was searched and why it came up empty.
- Show the per-check score table before the check details — founders need the summary before the breakdown.
- If a program has <3 angles with usable data, print this warning at the top: `⚠️ Limited public data — fewer than 3 checks could be fully evidenced. Verdict defaulted to Watch List regardless of score.`
- The Baloney Score is not a fit score — a fully legit program can still be wrong for this startup's gap or stage. Always clarify this before the recommended action.
- Total word budget for check details: ≤700 words. The score table and verdict rationale are not counted.
- Do not recommend `/incmatch` until the verdict is Legit or Probable.
- Never inflate a famous program's score on reputation alone — score the evidence for recent cohorts, not the brand's history.
