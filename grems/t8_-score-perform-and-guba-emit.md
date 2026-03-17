# T8\_ SCORE PERFORM & GUBA EMIT&#x20;

### T8: SCORE: PERFORM & GUBA EMIT

**GRAPE Rewards & Emissions Management System (GREMS)**\
Version 1.0 | Last Updated: 2025-12-30

Rewarding contributions, participation, and impact across the GRAPE DAO

### 1. Purpose

The **SCORE: PERFORM & GUBA EMIT** tab is the **monthly scoring and allocation engine** for Performance rewards. It records member work assignments and performance levels, then uses formulas to distribute monthly reward pools across three emission types:

* **PERFORM EMIT** → **GRAPE** rewards
* **GUBA EMIT** → **USDC** rewards
* **SOL EMIT** → **SOL** rewards

This tab is designed to minimize manual input and maximize auditability: **operators only select inputs**, while **all allocations are calculated automatically**.

**Control:** This tab supports **Performance (Monthly)** emissions only. It does **not** include **RECURRING PAYMENTS** sources (Proposal Architect refunds/rewards, Discord Boost, infra/tool costs) and does **not** include governance participation (VINE).

### 2. Operator Inputs (Manual / Dropdown Only)

Only the following fields should be edited by operators:

#### 2.1 Column A — Discord Name

* Selected from a dropdown list sourced from **GRAPE MEMBERS** (or equivalent roster tab).
* Ensures identity consistency (no typos, stable mapping).

#### 2.2 Column B — Task Name

* Selected from a dropdown list of approved monthly Performance tasks/categories (examples include):
  * Approved Proposal Architect
  * Call Documentation Upkeep (Discord / Recap)
  * Moderate (Discord / Calls)
  * Moderate (Discourse / Admin)
  * Product UAT DAO
  * Quality Control Quorum Committee
  * Creative Content & Videography
  * Product Updates – Code Pushes
  * Exploratory Committee
  * Partner Relationship Communications
  * Documentation: DAO & Community
  * Metrics: DAO (Track & Monitor)
  * Calendar/Event Management
  * Metrics: Syndicate (Track / Monitor / Update)
  * Workshop Organizer & Participants
  * Social Media (X/Twitter)
  * Discord Admin & Management
  * Research / Alpha / Tokens & NFTs
  * Treasury Controller & Emissions Execution
  * Community Coordinator

#### 2.3 Column C — Perform Level

* Selected from a dropdown of standardized levels:
  * **LEVEL 0 (0%)**
  * **LEVEL 1 (33%)**
  * **LEVEL 2 (66%)**
  * **LEVEL 3 (100%)**

**Operator rule:** Only update cells explicitly marked for input (commonly **yellow/green** fields in the template). Everything else is formula-driven.

_Figure 1. SCORE: PERFORM & GUBA EMIT — Operator Inputs (Discord Name, Task, Perform Level)_

### 3. Reference Outputs (Operator-Readable Totals)

These columns summarize the calculated results per row (and are used for rollups and downstream totals):

* **Column D — Total Rewards (GRAPE)** _(from PERFORM EMIT final)_
* **Column E — Total Allocation (USDC)** _(from GUBA EMIT final)_
* **Column F — Total Allocation (SOL)** _(from SOL EMIT final)_
* **Column G — Discord IDs** _(reference mapping)_
* **Column H — Task IDs** _(reference mapping)_
* **Column I — Blank / spacing (readability)**

These outputs make it easy to filter/sum rewards by member, by task, or by emission type.

### 4. Calculation Blocks (How Rewards Are Computed)

#### 4.1 PERFORM EMIT (GRAPE) — Columns J–P

_Figure 2. PERFORM EMIT (GRAPE) — Allocation Block and Totals_

This block allocates **GRAPE** from task pools.

* **J — Unique ID**\
  Combines Task ID + Discord ID (example: =H6&"\_"\&G6)
* **K — Perform Weight Score**\
  Converts LEVEL selection into a % via lookup (see Weight Table).
* **L — Graperator Count**\
  Counts members assigned to the same task with a valid (>0) weight score.
* **M — Task Pool Allocation (GRAPE)**\
  Pulls the monthly GRAPE pool for the task (source is the configured pool reference used by the system).
* **N — Perform Weight Reward**\
  Allocates the task pool proportionally to the member based on weight score and task participation.
* **O — Remaining Reward**\
  Tracks any unallocated remainder (after distribution).
* **P — Total GRAPE Rewards**\
  Final GRAPE reward for that row/task/member.

#### 4.2 GUBA EMIT (USDC) — Columns R–X

_Figure 3. GUBA EMIT (USDC) — Allocation Block and Totals_

This block allocates **USDC** from monthly USDC task pools.

* **R — Unique ID**\
  Task ID + Discord ID
* **S — Perform Weight Score**\
  LEVEL → % lookup
* **T — Graperator Count**\
  Counts members on the task
* **U — Task Pool Allocation (USDC)**\
  Pulls the USDC pool for the task (monthly config source)
* **V — Perform Weight Reward**\
  Proportional allocation by weight score and participation
* **W — Remaining Reward**\
  Leftover pool after allocation
* **X — Total Allocation (USDC)**\
  Final USDC reward for that row/task/member

#### 4.3 SOL EMIT (SOL) — Columns Z–AF

_Figure 4. SOL EMIT (SOL) — Allocation Block and Totals_

This block allocates **SOL** from the configured SOL task pools.

* **Z — Unique ID**\
  Task ID + Discord ID
* **AA — Perform Weight Score**\
  LEVEL → % lookup
* **AB — Graperator Count**\
  Counts members on the task
* **AC — Task Pool Allocation (SOL)**\
  Pulls the SOL pool for the task (monthly config source)
* **AD — Perform Weight Reward**\
  Proportional SOL allocation
* **AE — Remaining Reward**\
  Leftover pool after allocation
* **AF — Total Allocation (SOL)**\
  Final SOL reward for that row/task/member

### 5. Support Tables and Summaries

#### 5.1 Weight Level Lookup — Columns AH–AI

Defines the standardized mapping used across all three emission blocks:

* **AH — Weight Level:** LEVEL 0–3 (+ optional “Average”)
* **AI — Percentage:** 0%, 33%, 66%, 100% (and 50% for Average if used)

#### 5.2 Pivot / Summary (Task Rollups) — Columns AK–AN

A compact task-level rollup to help reviewers validate totals:

* **AK — Task Names**
* **AL — Sum of Perform**
* **AM — Sum of Remaining**
* **AN — Average of Perform**

This area is commonly used during review calls to confirm whether pools are distributing as expected.

_Figure 5 (Image 5: pivot/summary + weights area)_

### 6. Workflow (Monthly Use)

{% stepper %}
{% step %}
### During DAO call / review

Select **Discord Name (A)**, **Task (B)**, and **Perform Level (C)** for each row needed.
{% endstep %}

{% step %}
### Let formulas compute

GRAPE totals appear in **P**, USDC totals in **X**, SOL totals in **AF**.
{% endstep %}

{% step %}
### Review integrity

Use task summaries (AK–AN) and remaining columns (O/W/AE) to spot anomalies.
{% endstep %}

{% step %}
### Downstream rollups

Results are carried forward into the system’s payout-ready reference outputs and monthly reporting.
{% endstep %}
{% endstepper %}

_Figure 6. Performance Scoring Workflow — From DAO Call Inputs to REF Outputs_

### 7. Controls and System Alignment

* **Input-only discipline:** Operators should edit **only** dropdown input cells; formula blocks should remain unchanged.
* **Identity mapping:** All member selection should resolve through the canonical roster (**GRAPE MEMBERS**) to prevent payout mismatches.
* **Separation of artifacts:** This tab supports **Performance emissions** only and remains separate from **RECURRING PAYMENTS** and **Governance Participation** pipelines.
* **Monthly reporting alignment:** The **Emissions Results Interface (Performance)** rollup must be sourced from the Performance reference outputs (not from Recurring Payments sources).
