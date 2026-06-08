---
name: incmatch
description: "Match the active startup profile against a single entrepreneur-support program. Usage: /incmatch <Program Name>. Reads STARTUP_PROFILE.md, researches the program, and produces a structured Program Fit Report with 7-dimension fit scoring summing to /100, outcome-comp analysis, terms assessment, access path, and a clear Apply / Strengthen First / Skip recommendation. Saves to incmatch-<program>.md."
license: MIT
compatibility: Requires Claude Code with WebSearch and WebFetch. Internet connection required.
allowed-tools: WebSearch WebFetch Read Write
metadata:
  author: 3Flux
  version: "1.0"
  workflow-step: "4 — run after /incposer scores 60+"
---

# /incmatch — Program Fit Analysis

Match the active startup profile against a single program to answer one question: will it close *your* gap at terms you should accept? Produces a structured report with a 7-dimension fit score summing to /100, outcome-comp analysis, terms assessment, and the real path in.

## Invocation

```
/incmatch <Program Name>
```

Example: `/incmatch Techstars` or `/incmatch "Activate Fellowship"`

## Execution Steps

### Step 1 — Load Startup Context

Read `STARTUP_PROFILE.md` from the current working directory. Extract and hold in context:

- Company name and one-liner
- Founder names and key credentials
- Sector / category
- Stage (idea / prototype / MVP / revenue)
- Geography and relocation constraints
- The specific gap a program should close (from `/incstrat` if available, else infer)
- Whether the startup is capital-intensive or capital-light
- Whether IP is central
- Traction (exact metrics)
- Referral / network contacts (alumni, mentors, named connections)

Derive the program slug: lowercase, strip punctuation and spaces (`Y Combinator` → `y-combinator`).

If `STARTUP_PROFILE.md` does not exist, stop and tell the user: "Create `STARTUP_PROFILE.md` first — all /incmatch research is personalized to your startup."

### Step 2 — Baloney Pre-Check

Before researching, check for `incposer-<program-slug>.md` in the current directory.

- **Score < 40:** Stop. Tell the user: "This program scored [N]/100 on `/incposer` — likely baloney. Review the evidence or move to the next program."
- **Score 40–59:** Continue, but flag for a warning banner in the final report.
- **Score 60+ or file not found:** Proceed normally.

### Step 3 — Research the Program

Use WebSearch and WebFetch to gather evidence across these angles. Pull named graduates, real terms, and direct quotes.

1. `"<Program>" cohort curriculum what you get equity terms`
2. `"<Program>" alumni "<sector>" companies outcomes raised revenue`
3. `"<Program>" mentors investors demo day partners`
4. `"<Program>" application process acceptance rate selection criteria`
5. `"<Program>" founder review experience worth it`
6. `site:<program-website> apply OR program OR alumni`

### Step 4 — Score the 7 Fit Dimensions

Score each dimension on the evidence. Every score must cite specific evidence — no uncalibrated guesses.

| Dimension | Max Points | What it measures |
|-----------|-----------|------------------|
| Gap Fit | 25 | Does it close the *specific* gap the startup needs closed (not just "support")? |
| Stage Fit | 20 | Does it select for and serve founders at this exact stage right now? |
| Sector Fit | 15 | Sector-relevant mentors, curriculum, and alumni — or generic? |
| Terms Fit | 15 | Equity / fee / IP terms acceptable for the value delivered at this stage |
| Network & Access Fit | 10 | Does it provide the specific access (investors / customers / talent) the startup lacks? |
| Outcome Comps | 10 | Comparable graduates that demonstrably progressed |
| Logistics Fit | 5 | Timing, location/relocation, and format match founder constraints |

**Overall Fit Score:** sum of all 7. Maximum 100.

**Recommended Action thresholds:**
- **75–100 → Apply:** Strong gap fit, acceptable terms, viable path in. Proceed to `/incapply`.
- **50–74 → Strengthen First:** Good signal but a missing proof point, a weak term, or a missing access path. Close the gap, then return.
- **0–49 → Skip:** The program does not close this startup's gap, or the terms aren't worth it. Move on.

### Step 5 — Write Output and Report

Read the output template from `references/output-format.md`. Populate all sections using the synthesized evidence and scores.

Write the completed report to `incmatch-<program-slug>.md`.

The following strings must appear verbatim (downstream skills `incapply` and `incprep` parse them by exact match):
- `Recommended Action:` followed by `Apply`, `Strengthen First`, or `Skip`
- `Fit Score:` followed by the numeric score

Then tell the user:
- The Fit Score and Recommended Action
- If **Apply**: "Run `/incperks <program>` to audit the real value, then `/incapply incmatch-<program>.md` to draft the application."
- If **Strengthen First**: "The gap is [the lowest-scoring dimension]. Close it, then re-run `/incmatch`."
- If **Skip**: "This program won't close your gap — [primary reason]. Run `/inclist` for better-fit programs, or revisit `/incstrat` (no program may be the right call)."

---

## Output Rules

- Every score in the Fit Assessment must cite specific evidence — not "seems like a good fit"
- The Access Path section must name a specific route in (alum, mentor, open application with selection angle) — never generic
- Terms must be stated concretely (equity %, fee, IP terms) or marked `Unknown — verify directly`
- The Verdict table must include a row labeled exactly `Recommended Action`
- If outcome evidence for the last 3 cohorts is absent, cap Outcome Comps at 3/10 and note the data gap
- Each prose section should stay under 200 words; use tables and bullets for density
- Do not fabricate graduate company names, acceptance rates, or terms — cite sources or mark `Unknown`
- A famous program that doesn't close *this* startup's gap should score low on Gap Fit regardless of brand
