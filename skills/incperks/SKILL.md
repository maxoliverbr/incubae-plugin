---
name: incperks
description: "Audit the full value an entrepreneur-support program delivers beyond the badge — capital and credits, but also mentorship depth, investor and customer access, non-dilutive funding, brand signal, and alumni network — scored by relevance to the active startup, and netted against the equity, fee, and time it costs. Usage: /incperks <Program Name>. Saves to incperks-<program>.md."
license: MIT
compatibility: Requires Claude Code with WebSearch and WebFetch. Internet connection required.
allowed-tools: WebSearch WebFetch Read Write
metadata:
  author: 3Flux
  version: "1.0"
  workflow-step: "5 — run after /incmatch to weigh real value against real cost"
---

# /incperks — Program Value Audit

Programs sell the badge. This audits what's actually behind it. Produces a two-part value report: **Part A — Tangible Value** (capital, credits, services, with dollar estimates) and **Part B — Intangible Value** (mentorship depth, access, brand signal, alumni network, follow-on pathway). Both are scored by relevance to the active startup — and the report closes by **netting value against cost** (equity given up, fees, and founder time) so the founder sees whether the program clears its own price.

Use this after `/incmatch` to compare programs on total net value, not just prestige.

## Invocation

```
/incperks <Program Name>
```

Examples: `/incperks Techstars` or `/incperks "gener8tor"`

## Execution Steps

### Step 1 — Load Startup Context

Read `STARTUP_PROFILE.md`. Extract and hold in context:

- Stage — determines which benefits are usable now vs. later
- Sector and product category — determines whose mentorship and which customer access matters
- Team size and composition — determines recruiting/operational value
- Business model and GTM — determines whether customer intros or capital matters more
- Capital intensity — determines the weight of non-dilutive funding vs. equity investment
- The specific gap from `/incstrat` if available

If `STARTUP_PROFILE.md` does not exist, stop and tell the user to create it first.

Derive slug and filename: lowercase, strip spaces/punctuation. `Techstars` → `incperks-techstars.md`.

### Step 2A — Research Tangible Value

Use WebSearch and WebFetch across these angles:

1. `"<Program>" investment amount equity terms stipend non-dilutive`
2. `"<Program>" perks credits AWS GCP Azure deals partner offers`
3. `"<Program>" services legal accounting office space provided`
4. `"<Program>" demo day follow-on funding investors`
5. `site:<program-website> benefits OR perks OR what-you-get`

**Collect:** the headline capital (and its terms), partner-deal credits (AWS Activate, Google for Startups, Stripe, etc.), included services (legal, accounting, space), and any non-dilutive funding (grant, stipend, prize).

### Step 2B — Research Intangible Value

Use WebSearch and WebFetch across these angles:

6. `"<Program>" mentors named operators investors engagement`
7. `"<Program>" alumni network community founders help each other`
8. `"<Program>" brand signal credibility enterprise customers fundraising`
9. `"<Program>" customer introductions corporate partners pilots`
10. `"<Program>" follow-on investment alumni fund continued support`

**Collect:**
- Mentorship depth: named, relevant mentors and evidence they actually engage
- Investor access: who attends demo day / office hours and whether they write checks
- Customer access: corporate partners or customer intros the program brokers — critical for design-partner-hungry startups
- Brand signal: does the program's name move enterprise buyers, future investors, or talent in this sector?
- Alumni network: is it an active, helpful community or a mailing list?
- Follow-on pathway: alumni funds, pro rata, continued support after the cohort

**Source labeling:** mark each item **[Confirmed]** (program materials/official) or **[Reported]** (founder accounts, secondary sources).

**If fewer than 3 angles across both steps return usable data:** state this at the top and add: "Ask the program directly for their current benefits sheet and alumni outcomes — most maintain one but don't publish everything."

### Step 3 — Categorize, Score, and Net Against Cost

#### Part A — Tangible Value
Dollar estimate + relevance rating per item. **Relevance:** High = closes a named gap | Medium = useful, not urgent | Low = generic / not stage-appropriate.

Categories: **Capital** (investment + terms, stipend, prize) · **Credits & Tools** (cloud, dev, SaaS deals) · **Services** (legal, accounting, space, HR) · **Non-Dilutive Funding** (grants, matched funds).

#### Part B — Intangible Value
Impact rating + brief assessment per dimension. No dollar value — often the highest-leverage benefits.

1. **Mentorship Depth** — rate **Deep** (named, relevant, engaged operators) / **Relevant** (good general guidance) / **Thin** (logo wall). Name individuals.
2. **Investor Access** — rate **Strong** / **Moderate** / **Weak**. Who actually shows up and writes checks?
3. **Customer & Partner Access** — rate **Strong** / **Moderate** / **Weak**. Does it broker real pilots or design partners in this sector? *(Highest-leverage for early-revenue startups.)*
4. **Brand & Signal Value** — rate **High** / **Medium** / **Low** for this startup's specific buyers, future investors, and talent.
5. **Alumni Network** — rate **Active** / **Passive** / **Nominal**. Evidence of founders actually helping each other.
6. **Follow-On Pathway** — rate **Strong** / **Moderate** / **Unclear**. Alumni fund, pro rata, continued support.

#### Cost Side — what it takes
Quantify the price: equity % (and its dollar value at the startup's likely valuation), any fees, mandatory relocation/time, and IP terms. Convert founder time to an honest opportunity-cost note.

#### Net Verdict
State whether total value clears total cost **for this startup at this stage** — Clears Easily / Clears / Marginal / Underwater — with one sentence of reasoning.

### Step 4 — Produce the Value Report

Write `incperks-<program>.md` using the template.

---

## Output Format

When producing the output file, read the exact template from [references/output-format.md](references/output-format.md).

---

### Step 5 — Save and Route

Write the full report to `incperks-<program>.md`.

Then tell the user:
- "For most early-stage startups the intangibles — mentorship depth, customer access, brand signal — outweigh the credits. Weight them accordingly."
- "Check the Net Verdict: if value is Marginal or Underwater, the program isn't worth it even if it's legit. Negotiate the terms or walk."
- "Run `/incprep incmatch-<program>.md` to fold your Top Picks into the selection-interview talking points."

---

## Output Rules

**Tangible value:**
- **[Confirmed]** vs. **[Reported]** labels are mandatory for every row
- Estimated values must use a `~` prefix unless sourced from program materials
- If a category has no found value, write: "[Category]: None documented — verified against [sources searched]."

**Intangible value:**
- Every dimension must be assessed — write "Not documented — ask directly" rather than skipping
- Impact ratings are mandatory for every dimension
- Named individuals are required for Mentorship Depth — do not write "experienced mentors" without naming them
- Customer & Partner Access must reference the startup's specific buyer type
- Follow-On Pathway must note whether an alumni fund or pro rata exists

**Net verdict:**
- The cost side must include the dollar value of equity given up at the startup's likely valuation — not just the percentage
- Founder-time opportunity cost must be stated explicitly
- The Net Verdict (Clears Easily / Clears / Marginal / Underwater) is mandatory and must reference the startup's stage

**Top Picks & Questions:**
- Top Picks must draw from both parts and reference something specific from `STARTUP_PROFILE.md` — ≤150 words
- Questions section must include ≥3 specific items that could not be confirmed publicly
