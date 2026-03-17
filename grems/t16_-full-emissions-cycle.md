# T16\_ Full Emissions Cycle

**GRAPE Rewards & Emissions Management System (GREMS)**\
Version 1.0 | Last Updated: 2025-12-30

Rewarding contributions, participation, and impact across the GRAPE DAO

## 1. Purpose

This document provides a complete walkthrough of how GREMS moves from **approved budgets** and **validated scoring** to **payout-ready artifacts/proposals**. It is written for contributors, reviewers, and operators who want a clear, auditable view of what happens each month and each merit period.

GREMS supports **three parallel reward pipelines**:

* **Performance Rewards (Monthly)**
* **Merit Rewards (Period-Based)**
* **Governance Participation Rewards (Standalone payout artifact)**

**Control:** Governance Participation (VINE, raffles, eligibility signals) is processed in a standalone pipeline and **does not roll into emissions rollup reporting**.

## 2. Definitions

### 2.1 What “Emissions” Means in GREMS

In GREMS, “emissions” refers to rewards distributed from emission-based pools (typically **GRAPE, USDC, and SOL**) to eligible contributors based on validated scoring and budget constraints.

### 2.2 Key Output Artifacts

GREMS produces separate outputs that should not be consolidated:

* **EMISSIONS RESULTS – Performance** (artifact/proposals for monthly Performance totals)
* **EMISSIONS RESULTS – Merit** (artifact/proposals for period Merit totals)
* **RECURRING PAYMENTS** (monthly payment list for Proposal Architect + Discord Boost + infra/tool costs)
* **GOVERNMENT PARTICIPATION REWARDS** (standalone governance participation payout artifact)

## 3. Pools and Where They Belong

### 3.1 Emissions Results Interface (Rollup View)

The **Emissions Results Interface** tab is a rollup view of emission pools and summary metrics (budget, final rewards, remaining, max per member, utilization, validation).

Typical columns include:

* **PERFORM EMIT (GRAPE)**
* **GUBA EMIT (USDC)**
* **SOL EMIT (SOL)**
* **MERIT EMIT (GRAPE)**
* **GUBA MERIT (USDC)**
* **SOL other (SOL)** _(if configured; project-specific usage)_

**Control:** The Emissions Results Interface is a reporting/rollup surface. It does **not** include Recurring Payments sources (Proposal Architect, Discord Boost, infra/tool costs) and does **not** include governance participation (VINE).

### 3.2 What Each Pool Represents

Performance (Monthly)

* **PERFORM EMIT:** GRAPE rewards for monthly tasks/roles
* **GUBA EMIT:** USDC rewards tied to monthly performance categories
* **SOL EMIT:** SOL rewards tied to monthly performance categories

Merit (Period-Based)

* **MERIT EMIT:** GRAPE merit rewards (period-based recognition)
* **GUBA MERIT:** USDC merit rewards (period-based)

Other / Optional

* **SOL other:** Additional SOL distributions (only if explicitly configured and governed)

## 4. End-to-End Cycle

GREMS runs on two main cadences (plus a standalone governance participation payout stream):

* **Monthly cycle:** Performance + Recurring Payments
* **Merit cycle:** Period-based (often multi-month)
* **Governance participation:** Standalone (tracked and paid separately)

## 5. Performance Cycle (Monthly)

### 5.1 Inputs

* **Budget Pools per Category** (approved monthly budgets)
* **GRAPE MEMBERS** (canonical roster / identity mapping)
* **Monthly scoring inputs** (performance levels validated in open review)

### 5.2 Scoring and Calculation

{% stepper %}
{% step %}
QCG/community review confirms performance levels by category.
{% endstep %}

{% step %}
Operators enter monthly scoring in:

* **SCORE: PERFORM & GUBA EMIT**
{% endstep %}

{% step %}
The sheet calculates payout-ready totals across:

* GRAPE (PERFORM EMIT)
* USDC (GUBA EMIT)
* SOL (SOL EMIT)
{% endstep %}
{% endstepper %}

### 5.3 Reference Totals (Payout-Ready)

* **REF: PERFORM** = GRAPE totals per member (monthly)
* **REF: GUBA** = USDC + SOL totals per member (monthly)

### 5.4 Reporting and Proposals

* **REF: PERFORM + REF: GUBA → EMISSIONS RESULTS – Performance** (monthly rollup artifact)
* Operators typically generate **separate execution proposals** for:
  * GRAPE payout proposal (from REF: PERFORM)
  * USDC + SOL payout proposal (from REF: GUBA)

## 6. Merit Cycle (Period-Based)

### 6.1 Inputs

* **Budget Pools per Category** (approved budgets for the merit period)
* **GRAPE MEMBERS** (identity mapping)
* **Merit scoring decisions** (validated in open review)

### 6.2 Scoring Entry

{% stepper %}
{% step %}
Merit category definitions and scoring happens in:

* **SCORE: MERIT SCORING** _(category assignment + levels)_
{% endstep %}

{% step %}
Scoring data feeds into calculation in:

* **SCORE: MERIT EMIT & GUBA MERIT** _(no manual dropdowns here; it consumes the scoring sheet outputs)_
{% endstep %}
{% endstepper %}

### 6.3 Reference Totals (Payout-Ready)

* **REF: MERIT** = GRAPE totals per member (period)
* **REF: GUBA MERIT** = USDC totals per member (period)

### 6.4 Reporting and Proposals

* **REF: MERIT + REF: GUBA MERIT → EMISSIONS RESULTS – Merit** (period rollup artifact)
* Operators typically generate **separate execution proposals** for:
  * GRAPE payout proposal
  * USDC payout proposal

### 6.5 Control: Number of Months Scaling

Some merit pools are configured as **multi-month** cycles and use a **Number of Months** parameter in the Emissions Results Interface. In those cases:

* Displayed “Total Budget” may represent a **monthly budget**
* Period budget = **Total Budget × Number of Months**
* Utilization should be evaluated against the **period budget**

## 7. Governance Participation (Standalone)

Governance participation rewards are handled outside emissions rollups.

### 7.1 Inputs and Processing

* Member Participation & Eligibility Spreadsheets\
  → **GOV PARTICIPATION** (eligibility/voting/calls/raffles)\
  → **VINE Rewards** (derived from GOV PARTICIPATION outcomes)\
  → **GOVERNMENT PARTICIPATION REWARDS** (standalone payout artifact)

### 7.2 Control

Governance Participation rewards:

* **do not** feed Performance emissions reporting
* **do not** feed Merit emissions reporting
* are distributed via their own standalone artifact/proposals (often separate VINE vs GRAPE proposals)

## 8. Recurring Payments (Monthly Payment List)

Recurring Payments are not emissions rollups.

### 8.1 Sources

* **REF: PROPOSAL ARCHITECT** (SOL refunds + rewards)
* **REF: 2025 DISCORD BOOST** (USDC boost rewards)
  * **infrastructure/tool costs** (as applicable)

### 8.2 Output

* **REF: PROPOSAL ARCHITECT + REF: DISCORD BOOST (+ infra/tool costs) → RECURRING PAYMENTS**\
  **Control:** RECURRING PAYMENTS is a separate artifact/proposal from EMISSIONS RESULTS – Performance.

## 9. Validation and Review

Before publishing proposals, reviewers use the **Emissions Results Interface** and REF tabs to confirm:

* **Final Rewards** totals are consistent with REF sheets
* **Remaining** values make sense (budget caps enforced)
* **Max Emission per eligible member** caps are applied
* **Wallet count validation** checks pass where implemented
* Merit multi-month utilization is interpreted using **Number of Months** when applicable

## 10. GREMS Relationships

### 10.1 Performance (Monthly)

Budget Pools + GRAPE MEMBERS\
→ SCORE: PERFORM & GUBA EMIT\
→ REF: PERFORM + REF: GUBA\
→ EMISSIONS RESULTS – Performance (artifact)

### 10.2 Merit (Period)

Budget Pools + GRAPE MEMBERS\
→ SCORE: MERIT SCORING\
→ SCORE: MERIT EMIT & GUBA MERIT\
→ REF: MERIT + REF: GUBA MERIT\
→ EMISSIONS RESULTS – Merit (artifact)

### 10.3 Add-Ons (Monthly Recurring Payments)

REF: PROPOSAL ARCHITECT + REF: DISCORD BOOST (+ infra/tool costs)\
→ RECURRING PAYMENTS (artifact)

### 10.4 Governance Participation (Standalone)

Member Participation & Eligibility Spreadsheets\
→ GOV PARTICIPATION\
→ VINE Rewards\
→ GOVERNMENT PARTICIPATION REWARDS (artifact)

## 11. Common Issues and Controls

* **Member missing from payout:** confirm GRAPE MEMBERS roster entry and identifiers.
* **Unexpected zeros:** verify pool budgets and correct source REF tab mappings.
* **Validation failing:** check wallet count checks, formula ranges, and period scaling (Number of Months).
* **Remaining too high:** confirm eligible members were scored and the right pools are being evaluated (monthly vs period).

## 12. Summary

GREMS converts approved budgets and validated scoring into **payout-ready artifacts** with clear separation between:

* **Performance emissions (monthly)**
* **Merit emissions (period-based)**
* **Recurring Payments (monthly add-ons + infra/tool costs)**
* **Governance Participation rewards (standalone)**

This separation is a core control for auditability and proposal clarity.
