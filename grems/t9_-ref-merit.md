# T9\_ REF MERIT

**GRAPE Rewards & Emissions Management System (GREMS)**\
Version 1.0 | Last Updated: 2025-12-30

Rewarding contributions, participation, and impact across the GRAPE DAO

### 1. Purpose

The **REF: MERIT** tab consolidates **Merit-cycle GRAPE rewards** into a payout-ready, per-member format. It aggregates Merit scoring outputs from **SCORE: MERIT EMIT & GUBA MERIT**, applies caps and period scaling where applicable, and produces totals used for:

* **Merit payout proposals (GRAPE)**
* **Emissions Results Interface (Merit) rollup reporting**
* **Validation checks (wallet counts, candidate counts, pool utilization)**

Control: REF: MERIT is part of the **Merit (Period)** pipeline only. It does **not** include Performance (Monthly) rewards, Recurring Payments, or Governance Participation rewards.

### 2. Inputs and Dependencies

REF: MERIT is driven by these upstream sources:

* **SCORE: MERIT EMIT & GUBA MERIT** (primary source)
  * Provides the member-level Merit scoring outcomes that determine GRAPE allocations.
* **GRAPE MEMBERS** (identity source of truth)
  * Provides canonical mappings for **Discord Name → Discord ID → Wallet Address**.
* **Emissions Results Interface** (rollup parameters / caps)
  * Provides Merit-pool reporting totals and key parameters such as **Max Emission per Eligible Member** and (when used) **Number of Months** for period scaling.

### 3. Sheet Layout (What Each Section Does)

REF: MERIT is typically organized into four functional blocks:

#### 3.1 Merit Rewards Total Pivot Table (Columns A–C)

A Pivot Table consolidates Merit GRAPE totals by member:

* **Column A:** Discord Name
* **Column B:** Discord ID (QA)
* **Column C:** **Sum of Total Rewards** (calculated Merit GRAPE total)

This block is the **raw consolidated output** sourced from the scoring/emission calculations.

Figure 1. _Merit Rewards Total Pivot Table (REF: MERIT, Columns A–C)_

#### 3.2 Member Lookup and Final Rewards (Columns E–H)

This block turns the Pivot Table totals into a **payout-ready list** by mapping identity + wallets and calculating **final rewards**:

* **Column E:** Discord Name (selected from GRAPE MEMBERS)
* **Column F:** Discord ID (auto-filled from GRAPE MEMBERS)
* **Column G:** Wallet Address (auto-filled from GRAPE MEMBERS)
* **Column H:** **Final Rewards (Monthly / Period)** (final GRAPE payout amount)

Typical logic (high-level):

* Look up the member’s calculated reward total from the Pivot Table.
* Apply **Max Emission per Eligible Member** (cap), if configured.
* Return **0** if the member is not found or has no eligible rewards.

Figure 2. _Member Lookup + Final Rewards (REF: MERIT, Columns E–H)_

#### 3.3 Merit Category Pool Lookup (Columns J–M)

This section lists the Merit categories (or subcategories) used in the merit cycle and their pool sizing:

* **Merit List**
* **MERIT ID**
* **Total Pool** (per category/subcategory)
* **Total Pool (%)**

This block is used as a **reference table** for how the Merit pool is partitioned across categories.

Figure 3. _Merit List, IDs, and Pools (REF: MERIT, Columns J–M)_

#### 3.4 Summary Metrics (Final Rewards / Pool / Validation Box)

This block provides the at-a-glance rollup for the current Merit cycle:

* **FINAL REWARDS** (total GRAPE distributed)
* **TOTAL POOL** (GRAPE pool available for the merit cycle)
* **PRODUCTIVITY** (utilization rate)
* **MAX EMISSION per** (cap per eligible member)
* **TOTAL CANDIDATES** (count of eligible recipients)
* **AVG REWARD**
* **Number of Months** (if the merit cycle spans multiple months)

Important nuance (period scaling):

<details>

<summary>Period scaling explanation</summary>

If **Number of Months** is used for the Merit cycle, the “pool for the period” is commonly treated as:

* **Period Pool = (Monthly Pool) × (Number of Months)**

…and utilization/productivity should reflect the period pool when the cycle spans multiple months.

</details>

Figure 4. _Merit Summary Metrics (Final Rewards / Pool / Utilization / Caps / Number of Months)_

### 4. Data Flow Alignment (GREMS Merit Pipeline)

REF: MERIT sits in the **Merit (Period)** pipeline:

Budget Pools + GRAPE MEMBERS → SCORE: MERIT EMIT & GUBA MERIT → REF: MERIT → Emissions Results Interface (Merit) + GRAPE payout proposal

Control: REF: MERIT contributes to **Merit reporting** and **Merit payout proposals** only. It does **not** feed **Recurring Payments** or **Performance** reporting.

### 5. Operating Procedure (How to Run It)

{% stepper %}
{% step %}
### Confirm GRAPE MEMBERS is current

Verify Discord names, IDs, and wallet mappings in GRAPE MEMBERS.
{% endstep %}

{% step %}
### Confirm Merit cycle inputs are complete

Ensure SCORE: MERIT EMIT & GUBA MERIT contain the completed Merit cycle inputs.
{% endstep %}

{% step %}
### Refresh/confirm the Pivot Table

Refresh and confirm the Pivot Table in REF: MERIT (Columns A–C) to reflect the latest scoring outputs.
{% endstep %}

{% step %}
### Verify Member Lookup block

In the Member Lookup block (Columns E–H), ensure:

* Every eligible member is selected/mapped correctly.
* Wallets populate correctly.
* Final rewards reflect caps/parameters.
{% endstep %}

{% step %}
### Confirm Summary Metrics

Check that Summary Metrics match expectations (pool totals, candidate count, utilization).
{% endstep %}

{% step %}
### Prepare payout proposal and rollup

Use Final Rewards outputs to prepare the **Merit GRAPE payout proposal**, and ensure the **Emissions Results Interface (Merit)** rollup matches REF outputs.
{% endstep %}
{% endstepper %}

### 6. Controls and Validation Checks (Required)

* **Identity integrity:** all payouts must map through **GRAPE MEMBERS**.
* **Cap enforcement:** confirm Max Emission per Eligible Member is applied consistently.
* **Candidate count:** ensure **TOTAL CANDIDATES** matches the number of unique wallets receiving non-zero rewards.
* **Period scaling:** if **Number of Months** is used, confirm pool/utilization calculations reflect the intended period logic.
