# T5\_ Emission Results&#x20;

**GRAPE Rewards & Emissions Management System (GREMS)**\
Version 1.0 | Last Updated: 2025-12-30

Rewarding contributions, participation, and impact across the GRAPE DAO

## 1. Purpose

The **Emissions Results Interface** tab provides a single rollup view of **emission-based reward pools** for the active cycle (monthly or merit period). It allows reviewers and contributors to confirm, at a glance:

* Total budget allocated (and period scaling where applicable)
* Final rewards issued
* Remaining budget
* Validation / integrity checks
* Max emission per eligible member
* Pool utilization rates

Control: The Emissions Results Interface is an emissions rollup report. It excludes **RECURRING PAYMENTS** sources (Proposal Architect, Discord Boost, infra/tool costs) and excludes governance participation (VINE).

Figure 1. Emissions Results Interface (November 2025 GRIPv2 Emissions Templates)

Note: “Number of Months” is used to scale merit-period pools when the merit window spans multiple months.

## 2. Pools Represented in This Tab (By Column)

Each column represents a separate emission pool. Pools map to GREMS pipelines and their **REF source sheets**.

### 2.1 Performance (Monthly) Pools

PERFORM EMIT (GRAPE)

* Source of truth: **REF: PERFORM**
* Token: GRAPE
* Meaning: Monthly performance emissions tied to activity-based contributions.

GUBA EMIT (USDC)

* Source of truth: **REF: GUBA**
* Token: USDC
* Meaning: Monthly performance emissions paid in USDC (where configured).

SOL EMIT (SOL)

* Source of truth: **REF: GUBA**
* Token: SOL
* Meaning: Monthly performance emissions paid in SOL (where configured).

### 2.2 Merit (Period) Pools

MERIT EMIT (GRAPE)

* Source of truth: **REF: MERIT**
* Token: GRAPE
* Meaning: Merit-period emissions based on merit scoring for the defined period window.

GUBA MERIT (USDC)

* Source of truth: **REF: GUBA MERIT**
* Token: USDC
* Meaning: Merit-period emissions paid in USDC for the defined period window.

### 2.3 Optional / Template-Dependent Pools

SOL other (SOL)

* Optional pool shown in some templates. If used, it must be explicitly defined in-template and should not be confused with RECURRING PAYMENTS.

## 3. Emissions Pool Summary (Top Section)

This section summarizes budget and distribution totals for each pool.

### 3.1 Metrics and Meaning

Total Budget

* Total tokens allocated to the pool for the cycle.
* In templates using a multi-month merit period, merit pools may display a **monthly budget**, scaled by the **Number of Months** parameter.

FINAL REWARDS

* Total tokens distributed to eligible members from the pool.

Remaining

* Unused budget after distributions.

Validation

* Integrity checks to confirm totals and constraints are consistent with the pool’s rules and source totals.

Number of Months (template parameter)

* Used when the cycle spans multiple months (commonly for merit periods).
* For merit-period pools, the **period budget** is calculated as:

Period Budget = Total Budget × Number of Months

## 4. Max Emission Per Eligible Member (Middle Section)

This section shows a diagnostic maximum per eligible member (cap view), based on pool budgets and eligibility counts.

### 4.1 Metrics and Meaning

* Total Budget: The pool amount used for the max-per-eligible calculation.
* FINAL REWARDS: The realized distribution shown for comparison.

Control: This is a diagnostic limit view. Payout truth remains the REF totals.

## 5. Emissions Pool Utilized (Bottom Section)

This section shows how much of each pool was used.

### 5.1 Utilization Logic

* Performance (Monthly) pools:\
  Utilized = Final Rewards ÷ Total Budget
* Merit (Period) pools when Number of Months applies:\
  Utilized = Final Rewards ÷ (Total Budget × Number of Months)

## 6. Source-of-Truth Controls (GREMS-Aligned)

### 6.1 Performance Rollup Sources

The Emissions Results Interface (Performance pools) can only be derived from:

* **REF: PERFORM (GRAPE totals)**
* **REF: GUBA (USDC + SOL totals)**

### 6.2 Merit Rollup Sources

The Emissions Results Interface (Merit pools) can only be derived from:

* **REF: MERIT (GRAPE totals)**
* **REF: GUBA MERIT (USDC totals)**

### 6.3 Controls and Prohibitions

* Do not mix pipelines: Governance Participation (VINE) and RECURRING PAYMENTS sources must not appear in the Emissions Results Interface.
* No manual overrides: Totals must match computed REF outputs exactly. If the spreadsheet does not show it, the proposal must not include it.
