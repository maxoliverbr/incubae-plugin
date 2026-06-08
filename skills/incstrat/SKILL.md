---
name: incstrat
description: "Decide whether you need an entrepreneur-support program at all, and if so which kind. Usage: /incstrat. Reads STARTUP_PROFILE.md and any inclist.md or incmatch-*.md files, then produces an honest program strategy: do-you-even-need-one verdict, program-type fit (accelerator vs. incubator vs. venture studio vs. non-dilutive grant program vs. none), sequencing, and this week's next actions. Saves to incstrat.md."
license: MIT
allowed-tools: Read Write Bash(find:*) Bash(ls:*)
metadata:
  author: 3Flux
  version: "1.0"
  workflow-step: "1 — run first to decide whether a program is worth your time"
---

# /incstrat — Program Strategy Memo

Most founders join programs reflexively — for the logo, the cohort, the demo day — without asking whether the program closes a real gap or just consumes the scarcest resource a startup has: founder time. This skill forces that question first.

Synthesize everything known about the startup into a program strategy: what gap (if any) a program should close, which *type* of program fits, which to pursue, and what to do this week.

## Invocation

```
/incstrat
```

No argument needed. Reads from the current working directory.

## Execution Steps

### Step 1 — Load All Available Context

Read the following from the current working directory (load all that exist — do not error if some are missing):

1. `STARTUP_PROFILE.md` — **required**. If missing, stop and tell the user to create it first.
2. `inclist.md` — if exists, use for the program pipeline and tier breakdown
3. `incmatch-*.md` — load all program match reports; use for sequencing
4. `incposer-*.md` — if any exist, extract the Baloney Score and verdict for each program

### Step 2 — Produce `incstrat.md`

Write a 7-section program strategy memo. Be honest — the default answer to "should I do a program?" is often *no*. Lead with the gap, not the logo.

---

## Section 1: The Gap Test

One paragraph, brutal honesty. A program is only worth founder time if it closes a **specific, named gap** the founder cannot close faster alone. Assess:

- What the startup actually needs right now (capital, a specific customer channel, a technical co-founder, regulatory access, a first reference logo, structured deadlines, nothing)
- Whether a program is the *fastest* way to close that gap, or just the most visible one
- The opportunity cost: what 12 weeks of founder attention is worth at this stage
- One-sentence verdict: "A program is worth it **only if** it delivers [specific gap-closer]; otherwise the highest-leverage move is [build / sell / raise] directly."

## Section 2: Program-Type Fit

Not all "programs" are the same instrument. Score each type against the startup's actual stage and gap.

| Program Type | What it's for | Fit for this startup | Why |
|--------------|---------------|---------------------|-----|
| **Accelerator** (cohort, equity-for-cash, demo day) | Pre-seed/seed teams needing capital + network + forcing function | High / Medium / Low | [reason tied to profile] |
| **Incubator** (space, services, long horizon, often no equity) | Very early teams needing infrastructure and runway, not speed | High / Medium / Low | [reason] |
| **Venture studio** (co-build, large equity, hands-on) | Idea-stage founders without a full team | High / Medium / Low | [reason] |
| **Non-dilutive / grant program** (SBIR, DOE, state econ-dev, university) | Deep tech / capital-intensive teams with IP and milestones | High / Medium / Low | [reason] |
| **Fellowship / community** (no equity, network + credibility) | Founders needing signal or peer access more than cash | High / Medium / Low | [reason] |
| **No program** | Teams whose gap is execution, not access | — | [when "none" is the right answer] |

**Recommended instrument:** [the single best-fit type, or "none — here's why"].

## Section 3: What to Optimize For

Rank the three things this founder should demand from any program, in order. For each, name the proof you'd need to see before applying.

1. **[e.g., Non-dilutive capital / customer intros / technical credibility]** — proof to look for: [specific]
2. **[…]** — proof: [specific]
3. **[…]** — proof: [specific]

Then name the **dealbreakers** — what would make *any* program a net negative for this startup (e.g., >7% equity at this valuation, mandatory relocation, a demo-day-only program with no customer access, IP assignment clauses).

## Section 4: Sequencing Strategy

Answer three questions explicitly:

**Which program(s) first**: If an `inclist.md` exists, pull Tier 1. If `incmatch` reports exist, rank by fit score. Name them.

**Apply now vs. wait for a milestone**: Many programs are far easier to get into — and far more valuable — *after* a proof point (first LOI, working prototype, first revenue). State whether to apply now or after a specific milestone, and why.

**Parallel vs. focused**: Recommend one. Applying to many programs dilutes application quality and signals desperation; focusing risks a single point of failure. Give the rationale for this startup.

## Section 5: Milestone Map

Which proof points unlock which programs. Based on incmatch reports and inclist tiers if available.

| Milestone | Unlocks | Target date |
|-----------|---------|-------------|
| [Milestone 1 — e.g., working demo] | [Program(s) — names if known] | [Month Year] |
| [Milestone 2 — e.g., first paid pilot] | [Tier-1 accelerators] | [Month Year] |
| [Milestone 3 — e.g., IP filed] | [Non-dilutive / SBIR-style programs] | [Month Year] |

## Section 6: Risk Factors & Contingencies

For each risk: what it is, probability (high/medium/low), and the specific fallback.

If any `incposer-*.md` files were loaded, add a row for every program scoring below 60 — a Baloney-flagged program in your Tier 1 pipeline is a time-risk: coordination overhead, vanity demo day, or extractive equity with no offsetting value.

| Risk | Probability | Contingency |
|------|-------------|-------------|
| Program is a time sink with no real gap-closer | Medium | Apply only to programs scoring 60+ on `/incposer`; treat the rest as monitor-only |
| Best-fit program's batch timing doesn't match runway | Medium | Identify a rolling-admission or non-dilutive alternative as a bridge |
| Equity terms erode the cap table before the seed round | Medium | Cap total program equity given up; prefer non-dilutive or fellowship tracks |
| Founder time pulled off the one thing that matters | High | Define a weekly "program hours" ceiling; protect build/sell time |

## Section 7: This Week — Next 3 Actions

The three most important things to do in the next 7 days. Concrete and specific.

1. **[Action]**: [Exactly what to do, which program, what to draft] — deadline: [day]
2. **[Action]**: [Exactly what to do] — deadline: [day]
3. **[Action]**: [Exactly what to do] — deadline: [day]

---

## Output Rules

- Section 1 must seriously entertain "no program" as the answer — do not assume a program is needed
- The opportunity-cost framing must be explicit: program time is founder time taken from building and selling
- Every program name in Sections 4–5 must come from `inclist.md` or `incmatch-*.md` if those files exist — do not invent program names
- The milestone map dates must be consistent with `STARTUP_PROFILE.md` milestones
- This Week actions must be doable in 7 days by the founders with no additional research needed
- After saving, tell the user: "Run `/inclist` to research candidate programs, then `/incposer <program>` on each Tier 1 before committing time."
