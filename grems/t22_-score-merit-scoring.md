# T22\_  SCORE MERIT SCORING

{% stepper %}
{% step %}
### Purpose

The **SCORE: MERIT SCORING** tab is the **input and scoring interface** used during the **Merit (Period)** cycle (commonly quarterly) to assign **Merit performance levels** to contributors across defined Merit categories.

This tab is designed so reviewers and contributors can clearly understand:

* **What each Merit category means** (category descriptions are visible in the tab)
* **How the Merit pool is weighted** by category (pool size + percentage share)
* **How scores map to downstream rewards** via the Merit emissions calculation tabs

Control: This tab is a **scoring input sheet only**. It **does not calculate payouts**. It feeds downstream Merit calculation tabs that produce payout totals (e.g., **SCORE: MERIT EMIT & GUBA MERIT** and corresponding **REF** totals).
{% endstep %}

{% step %}
### Where This Tab Fits in GREMS (Merit Pipeline)

Within the **Merit (Period)** pipeline:

* Reviewers assign **Discord Name + Category + Performance Level** in **SCORE: MERIT SCORING**
* These inputs flow into the Merit calculation layer (e.g., **SCORE: MERIT EMIT & GUBA MERIT**)
* The calculation layer produces final totals that roll up into **EMISSIONS RESULTS – Merit (Period Rollup Report)** (the reporting artifact)

Control: Merit scoring is **separate** from:

* **Performance (Monthly)** scoring (SCORE: PERFORM & GUBA EMIT)
* **Recurring Payments** (Proposal Architect / Discord Boost / infra-tool costs)
* **Governance Participation** (VINE / participation outcomes)
{% endstep %}

{% step %}
### Category Definitions and Total Pools (Top Summary Table)

At the top of the tab, a structured “Merit Categories” table provides context and weighting.

Typical columns include:

* **Category** — High-level grouping (e.g., _Excellence, Asset/Access Growth, Tools & Innovation, Operations & Execution, Community Growth_)
* **Code** — Unique code for the category (e.g., **GMERIT0–GMERIT4**)
* **Coverage** — Plain-language description of what the category includes
* **Total Pool** — The pool allocated to that category for the period
* **Total Pool (%)** — The category’s share of the overall Merit pool

Control (Units): In GREMS documentation, Merit outputs are typically represented as:

* **MERIT EMIT** (often **GRAPE**)
* **GUBA MERIT** (often **USDC**)

If the category pool table is labeled “USDC” in an older template, confirm whether the table is expressing **category weight values** (relative) versus the actual **token unit** used in the Merit emission pool for that period.
{% endstep %}

{% step %}
### Merit Category Breakdowns (Detailed Sections)

Below the top summary, each major category expands into a breakdown showing:

* Subcategories (codes like **101, 201, 301**, etc.)
* Subcategory descriptions (what qualifies)
* Allocated pool values (and percent shares)
* A dedicated **input table** where reviewers assign performance levels to members

Recommended section order (as commonly seen in templates):

**Excellence (GMERIT0)**

Rewards exceptional “above and beyond” contributions across any area.

**Asset / Access Growth (GMERIT1)**

Focused on strengthening the DAO through financial growth, assets, mentorship, opportunities, and membership growth.

Subcategory examples:

* Mentorship (101)
* Treasury Growth (102)
* Treasury Growth – Other Assets (103) _(legacy)_
* Resource Size / Assets / NFTs (104)

**Tools & Innovation (GMERIT2)**

Recognizes tool creation, tool adoption/usage, and research contributions.

Subcategory examples:

* Tool Building (201)
* Tool Usage (202)
* Research (203)

**Operations & Execution (GMERIT3)**

Recognizes dependable operational support, reliability, and consistent execution.

Subcategory examples:

* DAO Doer (301)
* Consistency (302)

**Community Growth (GMERIT4)**

Recognizes brand presence, opportunity creation, and membership growth/retention.

Subcategory examples:

* Brand Recognition (401)
* Opportunity Creation (402)
* Community Growth + Active Members (403)
{% endstep %}

{% step %}
### Member Input Tables (Live Scoring During Calls)

Each category section contains a scoring table where reviewers enter the minimum required inputs:

* **Discord Name**
  * Selected via dropdown
  * Source: **GRAPE MEMBERS** roster
* **Category**
  * Selected via dropdown from the allowed Merit category/subcategory list
* **Performance Level**
  * Selected via dropdown (LEVEL 1, LEVEL 2, LEVEL 3, etc.)
  * Each level maps to a reward ratio used downstream

These inputs are typically entered **live during DAO calls**, where the QCG/community confirms which contributors earned which levels.
{% endstep %}

{% step %}
### Automation and Linkage (Downstream Calculation)

After inputs are entered:

* The **SCORE: MERIT SCORING** tab does **not** compute emissions totals directly
* Inputs are **copied automatically** into the Merit calculation tab(s), typically:
  * **SCORE: MERIT EMIT & GUBA MERIT**

That calculation layer applies:

* Approved budgets / pool weights
* Level ratios
* Eligibility constraints (where applicable)

…and produces final member totals for reporting and proposals.

Control: Do not manually edit calculated totals in downstream tabs. Corrections should be made by updating the **inputs** here (Name / Category / Level).
{% endstep %}

{% step %}
### Input Rules and Controls (Best Practice)

* **Inputs allowed:** Discord Name, Category, Performance Level
* **No manual overrides:** All reward totals must be formula-derived downstream
* **Dropdown enforcement:** Use dropdowns to prevent typos and broken links
* **Identity mapping:** Member identifiers and wallets should map through **GRAPE MEMBERS** when needed for payouts
* **Auditability:** Keep the tab readable and consistent so scoring decisions are traceable by category, period, and contributor
{% endstep %}

{% step %}
### Result

This tab provides a **transparent, structured, and repeatable** method to evaluate Merit contributions, while keeping:

* Category definitions visible
* Pool weighting understandable
* Scoring decisions auditable
* Reward calculations isolated to the downstream emissions calculation tabs
{% endstep %}
{% endstepper %}
