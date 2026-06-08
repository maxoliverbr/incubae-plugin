---
name: inclist
description: "Build a prioritized list of entrepreneur-support programs to target from the active startup profile. Usage: /inclist. Reads STARTUP_PROFILE.md, researches the accelerator/incubator/grant landscape, and produces a tiered list of 12–20 programs ranked by fit with rationale and a suggested application angle. Saves to inclist.md."
license: MIT
compatibility: Requires Claude Code with WebSearch and WebFetch. Internet connection required.
allowed-tools: WebSearch WebFetch Read Write
metadata:
  author: 3Flux
  version: "1.0"
  workflow-step: "2 — run after /incstrat to research the program landscape"
---

# /inclist — Program Target List Builder

Research the entrepreneur-support landscape — accelerators, incubators, venture studios, fellowships, and non-dilutive grant programs — and produce a ranked pipeline of 12–20 programs tiered by fit with the active startup profile.

## Invocation

```
/inclist
```

No argument needed — reads `STARTUP_PROFILE.md` from the current working directory.

## Execution Steps

### Step 1 — Load Startup Context

Read `STARTUP_PROFILE.md`. Extract and hold in context:

- Sector and category (deep tech, energy, climate, AI, SaaS, hardware, etc.)
- Stage (idea / prototype / MVP / revenue) and what gap a program would close
- Business model and whether it's capital-intensive (hardware/deep tech) or capital-light (software)
- Geography (and whether the founder can/will relocate)
- Whether IP is central (patents, research spin-out) — points toward non-dilutive and university programs
- Traction signals (revenue, pilots, grants, government partnerships)
- Team composition and any gap a venture studio or incubator could fill
- Whether the founder needs capital, customers, credibility, co-founders, or a forcing function most

If `STARTUP_PROFILE.md` does not exist, stop and tell the user to create it first.

### Step 2 — Research the Program Landscape

Use WebSearch (and WebFetch on program websites) to identify programs that match. Run searches across multiple angles:

1. Sector-specific accelerators: `accelerator "<sector>" cohort equity application 2025 2026`
2. Stage-matched programs: `pre-seed accelerator OR incubator "<sector>" non-dilutive 2026`
3. Non-dilutive / grant programs: `<sector> SBIR STTR OR DOE OR NSF OR state grant program startup`
4. University / regional programs: `<geography> university venture program OR incubator OR economic development startup`
5. Vertical / mission programs: `"<sector>" fellowship OR studio deep tech OR climate OR hard tech founders`

For each promising program found, quickly verify:
- Stage and sector match
- Whether it is currently running cohorts (last batch within ~18 months)
- Equity / cost structure (equity-for-cash, fee-based, free, grant)
- Whether it offers what the founder actually needs (capital vs. customers vs. credibility)

### Step 3 — Score and Tier Each Program

Rank programs across five dimensions:
1. **Gap fit** — does it close the specific gap from `/incstrat` (not just "looks prestigious")?
2. **Stage fit** — is the founder at the stage this program selects for, right now?
3. **Cost of capital** — equity/fee given up vs. value delivered (non-dilutive ranks higher, all else equal)
4. **Cohort comps** — has it graduated comparable companies that actually progressed?
5. **Access path** — is there a warm referral, alum, or mentor connection available?

### Step 4 — Produce `inclist.md`

Write the output to `inclist.md` in the current working directory using this structure:

---

```markdown
# Program Target List: [Startup Name]
_Generated: [date] | Stage: [stage] | Primary gap to close: [gap]_

---

## Tier 1 — Apply Now
*Strong gap fit + correct stage + acceptable terms. Prioritize the application within 2 weeks.*

### [Program Name]
- **Type**: [Accelerator / Incubator / Venture studio / Grant / Fellowship]
- **Terms**: [equity % + cash, fee, or non-dilutive — be specific]
- **Why**: [2–3 bullets — the specific gap it closes, a comparable graduate, or mentor/investor access]
- **Access angle**: [open application / referral via [name] / alum intro / mentor nomination]
- **Run /incposer first**: Yes / Already done (link if exists)

[Repeat for 4–6 programs]

---

## Tier 2 — Strengthen First
*Good fit but you'd be under-competitive now, or terms need a proof point to justify. Apply after [milestone].*

### [Program Name]
- **Type**: [type] · **Terms**: [terms]
- **Why**: [2–3 bullets]
- **What to wait for**: [specific milestone before applying]

[Repeat for 4–6 programs]

---

## Tier 3 — Monitor
*Stage mismatch, weak terms, or unclear value. Revisit later or skip.*

### [Program Name]
- **Why eventually**: [1–2 bullets]
- **Blocker**: [what would need to change]
- **Revisit when**: [milestone or date]

[Repeat for 3–5 programs]

---

## Pipeline Summary
- Total programs identified: [N]
- Tier 1 (apply now): [N]
- Non-dilutive options in pipeline: [N] — *(these protect your cap table; weight them up)*
- Application deadlines in next 90 days: [list program → deadline]
- Warm access paths available: [list names and who connects them]
- Suggested first application order: [Program 1] → [Program 2] → [Program 3]
```

---

### After Producing `inclist.md` — Baloney Pre-Filter

Once the list is written, tell the user to run `/incposer <program name>` on every Tier 1 program **before** committing application time:

> "Before running `/incmatch` on any of these, run `/incposer <program name>` to verify the program actually serves founders and isn't a vanity or coordination-overhead program. Programs scoring below 40 should be dropped. Programs scoring 40–59 should move to Tier 2 (only worth it with a warm path). Only proceed to `/incmatch` for programs scoring 60 or above."

---

## Output Rules

- Every program must include its real terms (equity, fee, or non-dilutive) — vague "supports startups" is not enough
- Cite the source of program/cohort information inline (e.g., "per their 2025 cohort page")
- Prefer non-dilutive and customer-access programs over demo-day-only accelerators when stage and gap are equal — note the reasoning
- Do not include programs with obvious stage mismatches unless there is a specific strategic reason noted
- Warm access paths take priority in the suggested order — always surface them
- Flag if any Tier 1 program already has an `/incmatch` report saved in the current directory
