# T19\_ Relationships (REF\_ MERIT + REF\_ GUBA MERIT + SCORE Merit)

**GRAPE Rewards & Emissions Management System (GREMS)**\
Version 1.0 | Last Updated: 2025-12-30

Rewarding contributions, participation, and impact across the GRAPE DAO

## 1. Purpose

This document explains how the **Merit-cycle** scoring and reference tabs work together to calculate and prepare **Merit-period emissions**:

* **MERIT EMIT (GRAPE)** payouts (via **REF: MERIT**)
* **GUBA MERIT (USDC)** payouts (via **REF: GUBA MERIT**)
* **Emissions Results Interface – Merit** totals + validation (rollup reporting)

**Control:** This document covers the **Merit (Period)** pipeline only. It does **not** include:

* Performance (Monthly) rewards (PERFORM / GUBA / SOL)
* Recurring Payments (Proposal Architect, Discord Boost, infra/tools)
* Governance Participation rewards (VINE)

## 2. Where This Fits in GREMS

Merit is a **period-based** cycle (commonly quarterly). The Merit pipeline follows this logic:

* **SCORE: MERIT SCORING** is the _scoring input sheet_ (review decisions).
* **SCORE: MERIT EMIT & GUBA MERIT** is the _calculation engine_ (converts scoring → payouts).
* **REF: MERIT** and **REF: GUBA MERIT** are the _payout-ready outputs_ (per member).
* **Emissions Results Interface – Merit** is the _rollup + validation interface_ used for reporting and proposal reconciliation.

## 3. Data Flow Alignment

This is the end-to-end Merit flow used in GREMS.

Figure 0. MERIT (Period) Pipeline — Data Flow (Identity, Budget Constraints, Member Totals, Proposals)

### 3.1 What each arrow label means (in practice)

* **Budget Constraints:** Approved budgets constrain category pool totals (no manual overrides).
* **Identity Validation:** Scoring inputs are constrained to valid members (Discord name/ID integrity).
* **Identity Map (Discord ID + Wallet):** All payout outputs map through **GRAPE MEMBERS** to ensure correct wallets.
* **Scoring Inputs:** Review decisions (name + merit type + performance level) feeding the calc engine.
* **Member Totals:** Per-member totals produced by the calc engine and consumed by REF tabs.
* **Totals + Validation:** Rollup totals and consistency checks used by the Emissions Results Interface.
* **Payout List:** Final per-wallet payout outputs used to create proposals.

## 4. Tabs Covered in This Document

| Tab                                      | Role in Merit Pipeline                     | Output Type                    |
| ---------------------------------------- | ------------------------------------------ | ------------------------------ |
| **SCORE: MERIT SCORING** (T22)           | Scoring input interface (review decisions) | Member/category/level inputs   |
| **SCORE: MERIT EMIT & GUBA MERIT** (T11) | Calculation engine (GRAPE + USDC)          | Row-level + totals             |
| **REF: MERIT** (T9)                      | Payout-ready GRAPE totals by member        | GRAPE payout list              |
| **REF: GUBA MERIT** (T10)                | Payout-ready USDC totals by member         | USDC payout list               |
| **Emissions Results Interface – Merit**  | Rollup reporting + validation              | Totals, remaining, utilization |

**Control:** Merit outputs in GREMS are **GRAPE + USDC only** (no SOL in the Merit pipeline).

## 5. How the Tabs Connect (Merit Cycle Logic)

### 5.1 Scoring layer (inputs)

**SCORE: MERIT SCORING** is where reviewers enter/confirm Merit scoring decisions:

* Discord Name
* Merit Category / Merit Type
* Performance Level (Level 0–3 or equivalent weights)

Those decisions are the only “human input” source for Merit scoring.

### 5.2 Calculation layer (emissions engine)

**SCORE: MERIT EMIT & GUBA MERIT** converts the scoring inputs into:

* **MERIT EMIT (GRAPE)** calculations
* **GUBA MERIT (USDC)** calculations

**Control (per T11):** This tab is **not** where scoring decisions are made; it is formula-driven from **SCORE: MERIT SCORING**.

### 5.3 Reference layer (payout-ready totals)

The REF tabs convert calculated outputs into payout-ready lists:

* **REF: MERIT** consolidates GRAPE totals per member and prepares the GRAPE payout list.
* **REF: GUBA MERIT** consolidates USDC totals per member and prepares the USDC payout list.

Both REF tabs enforce identity mapping through **GRAPE MEMBERS** (Discord ID + Wallet).

### 5.4 Rollup and validation layer

The **Emissions Results Interface – Merit** pulls totals from REF outputs to support:

* rollup reporting (final totals, remaining, utilization)
* validation checks (candidate counts, totals reconciliation)

## 6. Operator Run Order (Merit Cycle)

{% stepper %}
{% step %}
### Confirm budgets

Confirm budgets are approved for the period (Budget Pools per Category).
{% endstep %}

{% step %}
### Confirm GRAPE MEMBERS

Confirm GRAPE MEMBERS is current (Discord IDs + wallet addresses).
{% endstep %}

{% step %}
### Enter/finalize scoring

Enter/finalize scoring in **SCORE: MERIT SCORING** during the open review window.
{% endstep %}

{% step %}
### Verify calculation updates

Verify **SCORE: MERIT EMIT & GUBA MERIT** updates correctly (GRAPE + USDC totals).
{% endstep %}

{% step %}
### Verify REF outputs

Verify payout outputs in:

* **REF: MERIT** (GRAPE payout list)
* **REF: GUBA MERIT** (USDC payout list)
{% endstep %}

{% step %}
### Verify rollup totals + validation

Verify rollup totals + validation in **Emissions Results Interface – Merit**.
{% endstep %}

{% step %}
### Create proposals

Create **two proposals** (separate artifacts):

* **Proposal (GRAPE)** from **REF: MERIT** payout list
* **Proposal (USDC)** from **REF: GUBA MERIT** payout list
{% endstep %}
{% endstepper %}

## 7. Controls and Validation (Required)

* **Separation of concerns:** Merit is period-based and separate from Performance + Recurring Payments + Governance Participation.
* **Identity integrity:** All payouts must map through **GRAPE MEMBERS** (Discord ID + Wallet).
* **No manual overrides:** Reward totals must be formula-derived from approved budgets + scoring inputs.
* **Candidate count checks:** “Total Candidates” should match unique recipients receiving non-zero rewards.
* **Period scaling:** If “Number of Months” is used, confirm pool/utilization logic reflects the intended period pool.
