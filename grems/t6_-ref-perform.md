# T6\_ REF PERFORM

**GRAPE Rewards & Emissions Management System (GREMS)**\
Version 1.0 | Last Updated: 2025-12-30

Rewarding contributions, participation, and impact across the GRAPE DAO

## 1. Purpose

The **REF: PERFORM** tab consolidates and finalizes **PERFORM EMIT (GRAPE)** rewards for each DAO member, using performance inputs produced upstream in **SCORE: PERFORM & GUBA EMIT**.

This tab exists to produce a **payout-ready, per-member GRAPE total** that is:

* Traceable to the SCORE inputs (task + level selections)
* Capped by the maximum emission rules (per member)
* Validated against wallet counts (integrity control)

**Control:** REF: PERFORM supports **Performance emissions only** (GRAPE). It is not a RECURRING PAYMENTS source and does not include governance participation (VINE).

## 2. Inputs and Dependencies

REF: PERFORM depends on the following sources:

### 2.1 Upstream Calculation Source

* **SCORE: PERFORM & GUBA EMIT**
  * Uses the **PERFORM EMIT range (J–P)** as the source for total GRAPE outcomes per member.

### 2.2 Canonical Identity Source

* **GRAPE MEMBERS (Roster / Identifiers)**
  * Supplies Discord Name → Discord ID → Wallet Address mappings via dropdown selection.

### 2.3 Rollup/Cap References

* **Emissions Results Interface** (tab name)
  * Provides the **Total Pool** and **Max Emission per Eligible Member** values used by REF: PERFORM metrics and caps.

Important naming control: Any formulas that currently reference 'EMISSIONS RESULTS'!… should be updated to reference the **Emissions Results Interface** tab name to avoid confusion with system-level artifacts.

## 3. Tab Layout: Major Sections

REF: PERFORM is organized into four functional regions. The sections below describe each region and its purpose.

{% stepper %}
{% step %}
### 3.1 A–C: Summary Pivot Table (Calculated)

A Pivot Table aggregates SCORE outputs into a per-member total.

* **Column A — Discord Name** (from SCORE)
* **Column B — Discord ID** (from SCORE)
* **Column C — Total GRAPE Reward Calculated**
  * The aggregate GRAPE reward outcome for the member (pre-cap).

Purpose: Provide a single consolidated “source total” per member derived from SCORE.

_**Figure 1:**_ _A–C Pivot (Discord Name / Discord ID / Total Reward Calc)_
{% endstep %}

{% step %}
### 3.2 E–J: Member Details + Final Rewards + Metrics (Payout-Ready)

This section is the **operator-facing, payout-ready view**.

* **Column E — Discord Name**
  * Selected via dropdown from **GRAPE MEMBERS**.
* **Column F — Discord ID (QA)**
  * Auto-filled from GRAPE MEMBERS based on Column E.
* **Column G — Wallet Address**
  * Auto-filled from GRAPE MEMBERS based on Column E.
* **Column H — Final Rewards (GRAPE)** _(capped)_
  * Caps the pivot total at the “Max Emission per Eligible Member” value.
* Example logic (as written):
  * Lookup the member’s calculated total (Pivot)
  * If below cap → use it
  * If above cap → apply cap
  * If lookup fails → 0
* **Column I — Number of Tasks**
  * Counts task rows in **SCORE: PERFORM & GUBA EMIT** for that member where the level is not Level 0 (0%).
* **Column J — Average Level %**
  * Extracts the percentage from the level strings (e.g., LEVEL 2 (66%)) and averages them for that member.

Purpose: This region is the **actual payout list** (Name, ID, Wallet, Final Rewards) plus performance context (task count, average level).

_**Figure 2:**_ _E–J Member lookup + Final Rewards + task metrics_
{% endstep %}

{% step %}
### 3.3 L–Q: Task List + Pool Ratios + Pool Calculations

This section defines task pools and ratios used for allocation and auditing.

* **Column L — Task List** _(manual list of tasks)_
* **Column M — Task ID** _(manual task IDs)_
* **Column N — Total Pool (per task)** _(calculated)_
  * Uses each task’s pool ratio × total pool.
* **Column P — Pool Ratio (Hidden, Manual)**
  * Approved ratios set by the DAO.
* **Column Q — Pool Ratio (Calculated / Normalized)**
  * Normalizes the manual ratio (example shown: dividing by 1.0335).

Control: Only update the manual ratio inputs if the DAO approves changes. The normalization factor should only be changed if the template standard changes.

_**Figure 3:**_ _L–Q Task list + Task IDs + pools/ratios + summary metrics panel_
{% endstep %}

{% step %}
### 3.4 R–U: Summary Metrics + Validation

This region summarizes totals and provides integrity checks.

Key Metrics (typical intent):

* **Final Rewards total**
* **Total Pool**
* **Productivity (utilization)**
* **Max emission per eligible member**
* **Total candidates**
* **Average reward**

Validation:

* Ensures that the number of rewarded wallets aligns with the count of unique wallets receiving non-zero rewards.
{% endstep %}
{% endstepper %}

## 4. Output

REF: PERFORM produces:

### 4.1 Primary Output

* **Final GRAPE rewards per member** (payout-ready), including:
  * Discord Name
  * Discord ID
  * Wallet Address
  * Final Rewards (GRAPE)

### 4.2 Downstream Usage

* Used as the **source table for Performance GRAPE proposals**
* Used as a **source of truth** feeding the **Emissions Results Interface** rollup reporting for Performance (GRAPE totals)

## 5. Data Entry Rules

### 5.1 Allowed Manual Actions

* Add/update member identity fields in **GRAPE MEMBERS**
* In REF: PERFORM:
  * Select member names via dropdown in Column E
  * Maintain the task list and task IDs (if the DAO changes the task catalog)
  * Update manual pool ratios **only** when approved

### 5.2 Prohibited / Avoid

* Do not manually edit Final Rewards cells (H) unless repairing a formula.
* Do not override pivot outputs—fix the upstream SCORE inputs instead.

## 6. Controls and Audit Checks

* **Cap enforcement:** Final rewards must not exceed the “Max emission per eligible member” value for the cycle.
* **Wallet integrity:** Wallet count validation must pass before proposals are generated.
* **Separation of concerns:** REF: PERFORM is **Performance/GRAPE only** and is not combined with:
  * RECURRING PAYMENTS sources (Proposal Architect / Discord Boost / infra costs)
  * Governance participation outputs (VINE)

## 7. REF: PERFORM Flowchart

_**Figure 4:**_ _REF: PERFORM flowchart_
