# T18\_ Relationships (REF\_ PERFORM + REF\_ GUBA + SCORE Performance)

**GRAPE Rewards & Emissions Management System (GREMS)**\
Version 1.0 | Last Updated: 2025-12-30

Rewarding contributions, participation, and impact across the GRAPE DAO

## 1. Overview

This document explains how the **SCORE: PERFORM & GUBA EMIT** tab works with the **REF** tabs to produce payout-ready outputs for the **Performance (Monthly) pipeline** in GREMS.

At a high level:

* **Grape Members** provides **identity + wallet mapping** (and the eligibility list used by scoring + payout sheets).
* **Budgets** constrain how much can be distributed in each pool.
* **SCORE: PERFORM & GUBA EMIT** performs the weighted calculations for:
  * **PERFORM EMIT (GRAPE)**
  * **GUBA EMIT (USDC)**
  * **SOL EMIT (SOL)**
* **REF: PERFORM** and **REF: GUBA** consolidate results into **payout lists**.
* **Emissions Results Interface** pulls totals for **rollup + validation**, and supports proposal totals verification.
* Final payouts are executed via **two proposals**:
  * **Proposal (GRAPE)**
  * **Proposal (USDC & SOL)**

## 2. Scope and Tabs Covered

This document covers the Performance pipeline tabs only:

|                                 |                                                                                              |
| ------------------------------- | -------------------------------------------------------------------------------------------- |
| **Tab**                         | **Role in the System**                                                                       |
| **Grape Members**               | Member identity source (Discord name/ID, wallet); eligibility list used by scoring + payouts |
| **Budgets**                     | Budget caps / pool totals that constrain emissions                                           |
| **SCORE: PERFORM & GUBA EMIT**  | Main calculation tab (weights → allocations) for GRAPE, USDC, SOL                            |
| **REF: PERFORM**                | Consolidated GRAPE payout list + task pool totals                                            |
| **REF: GUBA**                   | Consolidated USDC + SOL payout list + task pool totals                                       |
| **Emissions Results Interface** | Rollup/validation layer that references REF totals (and validation cross-checks)             |

**Out of scope**: Merit pipeline, governance participation pipeline, recurring payments tabs.

## 3. GREMS Data Flow (Performance Pipeline)

**Figure 0** shows the simplified GREMS flow for Performance:

* **Grape Members + Budgets → SCORE: PERFORM & GUBA EMIT → REF: PERFORM + REF: GUBA → Emissions Results Interface**
* **REF payout lists → Proposals**
* **Emissions Results Interface totals/validation → Proposal verification**

**Figure 0 (Flowchart):** _Performance Pipeline (PERFORM + GUBA + SOL)_

## 4. Key Concepts

### 4.1 What “REF” means in GREMS

REF tabs are **payout-ready consolidation layers**:

* They aggregate member totals into a clean list (name, ID, wallet, final reward).
* They apply caps (if configured).
* They summarize pool totals used by **Emissions Results Interface** and proposal preparation.

### 4.2 What “SCORE” means in GREMS

SCORE tabs are the **calculation engines**:

* Reviewers enter levels/scores.
* Formulas compute weights, allocations, and totals across pools.
* SCORE outputs are then consolidated into REF tabs.

### 4.3 Does SCORE use Grape Members verification?

**Yes.** SCORE depends on **Grape Members** for identity mapping (Discord name → Discord ID → wallet) and for controlling who can appear in scoring/payout lists. In practice:

* **Member identity fields** in SCORE are validated/filled via lookups from Grape Members.
* REF tabs also use Grape Members to ensure payouts map to the correct wallet.

## 5. TAB: SCORE: PERFORM & GUBA EMIT

### 5.1 Purpose

This is the main operational sheet where scoring inputs are transformed into token allocations for:

* **PERFORM EMIT (GRAPE)**
* **GUBA EMIT (USDC)**
* **SOL EMIT (SOL)**

### 5.2 What Reviewers Update

Reviewers only touch **input cells** (commonly highlighted). Typical inputs per row include:

* **Discord Name**
* **Task Name**
* **Perform Level** (LEVEL 0–3 or equivalent scoring)

Note: Your implementation uses dropdowns for selection in this sheet. (Other pipelines may use manual scoring sources, but this SCORE tab includes dropdown-driven inputs.)

### 5.3 Output Sections Inside SCORE

SCORE is typically organized into these blocks:

|                               |                                                 |
| ----------------------------- | ----------------------------------------------- |
| **Block**                     | **Output**                                      |
| **Input grid + grand totals** | Shows totals for GRAPE / USDC / SOL             |
| **PERFORM EMIT section**      | Weighted GRAPE calculations by member-task      |
| **GUBA EMIT section**         | Weighted USDC calculations by member-task       |
| **SOL EMIT section**          | Weighted SOL calculations by member-task        |
| **Weights + task summaries**  | Level weights, aggregated task totals, averages |

**Figure 1:** _SCORE Input Grid + Grand Totals (GRAPE/USDC/SOL)_

**Figure 2:** _SCORE PERFORM EMIT calculation block_

**Figure 3:** _SCORE GUBA EMIT calculation block_

**Figure 4:** _SCORE SOL EMIT calculation block_

**Figure 5:** _SCORE Weight Levels + Task Summary/Distribution table_

### 5.4 Controls

* **Budget constraints** should ensure totals do not exceed pool budgets.
* SCORE totals are later cross-checked by the **Emissions Results Interface** validation logic.

## 6. TAB: REF: PERFORM

### 6.1 Purpose

REF: PERFORM consolidates the **final GRAPE rewards per member** produced by SCORE into a payout list.

### 6.2 Outputs

REF: PERFORM produces:

* **Member payout list (GRAPE)**
  * Discord name
  * Discord ID
  * Wallet
  * Final GRAPE reward
  * Optional metrics (task count, average level)
* **Task pool summary totals**
* **Final totals** used by **Emissions Results Interface** and proposal prep

**Figure 6:** _REF: PERFORM tab layout (payout list + pool summaries + totals panel)_

### 6.3 How REF: PERFORM is used

* **Payout List → Proposal (GRAPE)** (execution artifact)
* **Totals → Emissions Results Interface** (rollup and validation)

## 7. TAB: REF: GUBA

### 7.1 Purpose

REF: GUBA consolidates **final USDC and SOL rewards per member** produced by SCORE into a payout list.

### 7.2 Outputs

REF: GUBA produces:

* **Member payout list (USDC + SOL)**
  * Discord name
  * Discord ID
  * Wallet
  * Final USDC reward
  * Final SOL reward
* **Task pool summary totals** (USDC + SOL)
* **Final totals** used by **Emissions Results Interface** and proposal prep

**Figure 7:** _REF: GUBA tab layout (USDC/SOL payout list + pool summaries + totals panel)_

### 7.3 How REF: GUBA is used

* **Payout List → Proposal (USDC & SOL)** (execution artifact)
* **Totals → Emissions Results Interface** (rollup and validation)

## 8. TAB: Emissions Results Interface (Role in Performance)

### 8.1 Purpose in this pipeline

The Emissions Results Interface is the **rollup + validation layer**. For the Performance pipeline, it:

* Pulls **distributed totals** from:
  * **REF: PERFORM** (GRAPE)
  * **REF: GUBA** (USDC + SOL)
* Compares distributed totals to budgets (Remaining)
* Performs validation cross-checks (where configured)

### 8.2 Key rule from GREMS

For Performance rollups, the Emissions Results Interface derives its totals from:

* **REF: PERFORM + REF: GUBA**\
  (not from SCORE directly)

## 9. Proposal Outputs (Execution Artifacts)

### 9.1 Two proposal model (required)

Performance payouts are executed via **two separate proposals**. Converted to a stepper for clarity:

{% stepper %}
{% step %}
### Proposal (GRAPE)

* Primary source: **REF: PERFORM payout list**
* Verified by: **Emissions Results Interface totals/validation**
{% endstep %}

{% step %}
### Proposal (USDC & SOL)

* Primary source: **REF: GUBA payout list**
* Verified by: **Emissions Results Interface totals/validation**
{% endstep %}
{% endstepper %}

This separation is a GREMS control: **do not consolidate these into a single proposal artifact**.

## 10. Common Issues and Troubleshooting

| **Issue**                       | **Likely Cause**                                              | **Fix**                                                                  |
| ------------------------------- | ------------------------------------------------------------- | ------------------------------------------------------------------------ |
| Member missing from payout list | Not present in **Grape Members** / ID-wallet mapping missing  | Add/update member in Grape Members; reselect in SCORE/REF                |
| Totals don’t match              | REF totals not refreshed / formulas broken / SCORE incomplete | Refresh pivots (if used), verify formula ranges, confirm all rows scored |
| Validation shows FALSE          | Mismatch between REF totals and expected totals               | Trace totals: SCORE → REF → Emissions Results Interface                  |
| Unexpected 0 rewards            | Member scored LEVEL 0 or lookup failure                       | Confirm SCORE inputs; confirm name matches and lookups resolve           |

## 12. Summary

The Performance emissions system uses **Grape Members + Budgets** as inputs to **SCORE: PERFORM & GUBA EMIT**, then consolidates results into **REF: PERFORM** (GRAPE) and **REF: GUBA** (USDC + SOL). The **Emissions Results Interface** pulls totals from REF tabs for rollup and validation. Final payouts are executed through **two separate proposals**: **Proposal (GRAPE)** and **Proposal (USDC & SOL)**.
