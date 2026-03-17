# T2\_ System Overview

**GRAPE Rewards & Emissions Management System (GREMS)**\
Version 1.0 | Last Updated: 2025-12-30

Rewarding contributions, participation, and impact across the GRAPE DAO.

## GRIPv2 Emissions & VINE Rewards Spreadsheets

Together, the **GRIPv2 Emission** and **VINE Rewards** spreadsheets form the operational foundation of GREMS (also referred to as the **Graperators Emissions Pool Management System**).

This collection of interconnected sheets enables the **DAO Quality Control Group (QCG)** and the broader community to review completed tasks, validate contributions, and confirm earned rewards. Reviews are open to any DAO member who attends regular meetings, supporting a transparent process. The spreadsheets share input data and reference tables to automatically calculate rewards based on approved budgets, task rules, participation level, attendance, engagement level, and cap constraints.

## 1. Document Purpose and Scope

### 1.1 Purpose

This document explains how data flows across GREMS and provides an operator-friendly procedure to execute monthly and period-based reward cycles with consistent, auditable outputs.

### 1.2 Primary Audience

Community members, contributors, and the general public who seek to understand how rewards are calculated, assembled for payment, and reported.

### 1.3 Scope

GREMS consists of four parallel reward pipelines:

* **Emission Results - Performance** (Monthly) Rewards
* **Recurring Payments (Monthly)** — Proposal Architect + Discord Boost + Infrastructure/Tool Costs
* **Emission Results - Merit** (Period) Rewards
* **Governance Participation Rewards** (VINE & GRAPE + other prizes)

## 2. System Components

### 2.1 Upstream Inputs (Source Data)

A) Budget Pools per Category

* Defines the approved reward budgets for each category (per cycle).
* Serves as the primary constraint for distribution calculations.

B) GRAPE MEMBERS (Member Roster / Identifiers)

* Canonical roster for identity mapping (e.g., handles, wallet addresses, unique identifiers).
* Ensures payout totals are attributed correctly and reduces mismatches.

C) Member Participation & Eligibility Spreadsheets

* External/parallel spreadsheets used to capture governance participation signals (e.g., eligibility, voting, calls, raffles).
* Used exclusively to produce governance participation outcomes and VINE reward derivations.

### 2.2 Core Calculation and Reference Sheets

#### Performance (Monthly)

* **SCORE: PERFORM & GUBA EMIT** — calculates **GRAPE / USDC / SOL** based on budgets and monthly performance levels.
* **REF: PERFORM** — tabulates final monthly **GRAPE totals** per member.
* **REF: GUBA** — tabulates final monthly **USDC + SOL totals** per member.

#### Recurring (Monthly)

* **REF: PROPOSAL ARCHITECT** and **REF: DISCORD BOOST** provide the data necessary to create monthly proposals rewarding proposal architects and Discord boosters.
* **REF: PROPOSAL ARCHITECT** — computes **SOL refunds + rewards** for proposal architects.
* **REF: DISCORD BOOST** — computes **USDC rewards** for Discord boosts.

#### Merit (Period)

* **SCORE: MERIT EMIT & GUBA MERIT** — calculates **GRAPE / USDC** based on budgets and period performance levels.
* **REF: MERIT** — tabulates final period **GRAPE totals** per member.
* **REF: GUBA MERIT** — tabulates final period **USDC totals** per member.

#### Governance Participation

* **GOV PARTICIPATION** — evaluates governance eligibility and participation outcomes (voting, calls, raffles).
* **VINE Rewards** — derived from GOV PARTICIPATION outcomes.

### 2.3 Payments and Reporting Outputs

The totals from these sheets serve as input for payment and reward proposals.

* **RECURRING PAYMENTS** — monthly consolidated payout instruction list (assembled from monthly REF sources + infrastructure and tool costs).
* **GOVERNMENT PARTICIPATION REWARDS** — governance participation payout artifact (standalone; not part of emissions rollups).
* **EMISSIONS RESULTS - Performance (Monthly Rollup Report)** — monthly reporting derived from performance payout totals.
* **EMISSIONS RESULTS - Merit (Period Rollup Report)** — period reporting derived from finalized merit payout totals.

[_GRAPE Rewards & Emissions Management System (GREMS)_](https://www.mermaidchart.com/view/6e573211-7f69-47c0-a81c-71795f1dd3c3)<br>

## 3. Data Flow: Performance (Monthly) Pipeline

### 3.1 Inputs to Monthly Scoring

* Monthly Budget Pools for the month → SCORE: PERFORM & GUBA EMIT\
  Approved budgets constrain monthly distribution for each category.
* Monthly performance levels (entered into SCORE) → SCORE: PERFORM & GUBA EMIT\
  During review, the QCG records each member’s monthly performance level in each category for the prior month.
* GRAPE MEMBERS → SCORE / REF sheets\
  Member identifiers map through GRAPE MEMBERS to ensure consistent identity attribution across payout outputs.

### 3.2 Calculation Output: Monthly Member Reward Totals

* SCORE: PERFORM & GUBA EMIT → REF: PERFORM\
  Produces final **GRAPE totals** per member in payout-ready format.
* SCORE: PERFORM & GUBA EMIT → REF: GUBA\
  Produces final **USDC + SOL totals** per member in payout-ready format.

### 3.3 Monthly Add-On Reward Streams

* REF: PROPOSAL ARCHITECT → RECURRING PAYMENTS\
  SOL refunds + rewards for proposal architect activities appear on the monthly payment list.
* REF: DISCORD BOOST → RECURRING PAYMENTS\
  USDC rewards for Discord boosts appear on the monthly payment list.

### 3.4 Consolidation and Monthly Reporting

* Performance emissions (monthly rollup)\
  REF: PERFORM + REF: GUBA → **EMISSIONS RESULTS – Performance (Monthly Rollup Report)**\
  These REF sheets consolidate monthly **Performance** payout totals only (GRAPE totals from REF: PERFORM and USDC + SOL totals from REF: GUBA).
* Recurring payments (monthly payment list)\
  REF: PROPOSAL ARCHITECT + REF: DISCORD BOOST (+ infrastructure/tool costs, if applicable) → **RECURRING PAYMENTS (Monthly Payment List)**\
  These sources consolidate monthly **Recurring Payment** totals only (SOL refunds + rewards, USDC boost rewards, and approved operating costs).
* Control: no consolidation between artifacts\
  RECURRING PAYMENTS and EMISSIONS RESULTS – Performance are **separate artifacts/proposals** and are not merged into a single execution list.

Note: Governance Participation rewards are tracked separately via **GOVERNMENT PARTICIPATION REWARDS** and do not flow into **EMISSIONS RESULTS – Performance** nor **RECURRING PAYMENTS**.

## 4. Data Flow: Merit (Period) Pipeline

### 4.1 Inputs to Period Scoring

* Monthly Budget Pools for period → SCORE: MERIT EMIT & GUBA MERIT\
  Approved budgets constrain period distributions for each category.
* Period performance levels (entered into SCORE) → SCORE: MERIT EMIT & GUBA MERIT\
  During review, the QCG records each member’s prior-period performance level per category.
* GRAPE MEMBERS → SCORE / REF sheets\
  Ensures correct identity mapping for merit-period payouts.

### 4.2 Calculation Output: Period Member Totals

* SCORE: MERIT EMIT & GUBA MERIT → REF: MERIT\
  Produces final **GRAPE totals** per member for the merit period.
* SCORE: MERIT EMIT & GUBA MERIT → REF: GUBA MERIT\
  Produces final **USDC totals** per member for the merit period.

### 4.3 Period Reporting

* REF: MERIT + REF: GUBA MERIT → **EMISSIONS RESULTS - Merit (Period Rollup Report)**\
  Period rollup reporting is derived from the finalized merit payout totals.

## 5. Data Flow: Governance Participation Pipeline (Standalone)

### 5.1 Participation Inputs

* Member Participation & Eligibility Spreadsheets → GOV PARTICIPATION\
  Tracking participation enables the calculation of governance eligibility and participation outcomes.

Note: Participation spreadsheets do not feed VINE Rewards directly.

### 5.2 VINE Reward Derivation

* GOV PARTICIPATION → VINE Rewards\
  Governance participation outcomes determine VINE reward totals.

### 5.3 Governance Payout Artifact

* GOV PARTICIPATION + VINE Rewards → GOVERNMENT PARTICIPATION REWARDS\
  Governance participation and VINE outputs consolidate into a payout-ready artifact/proposal.

Note: This artifact does not flow into either of the EMISSIONS RESULTS.

## 6. Recommended Operating Procedure (Run Order)

{% stepper %}
{% step %}
### Pre-Run Validation (Required)

* Confirm any updates to the Budget Pools per Category for the cycle.
* Confirm the GRAPE MEMBERS roster is up to date (identifiers and payout fields).
* When issuing governance rewards, update governance participation inputs in the Member Participation & Eligibility Spreadsheets.
* Confirm completion of QCG/community review before submitting any proposal to the community.
{% endstep %}

{% step %}
### Monthly Cycle Execution

* Update **SCORE: PERFORM & GUBA EMIT** with monthly performance levels.
* Review **REF: PERFORM** and **REF: GUBA** for completeness and consistency.
* Update **REF: PROPOSAL ARCHITECT** (if applicable).
* Update **REF: DISCORD BOOST** (if applicable).
* Update infrastructure and tool cost list (if applicable).
* Consolidate the outputs from **REF: PROPOSAL ARCHITECT and REF: DISCORD BOOST (+ any infrastructure and tool costs)** to create a single composite **RECURRING PAYMENTS** proposal paying/rewarding USDC and/or SOL.
* Consolidate the outputs from **REF: PERFORM** and **REF: GUBA** into **EMISSIONS RESULTS (Performance)** to create two separate proposals: one to reward GRAPE and another to reward USDC & SOL.
{% endstep %}

{% step %}
### Merit (Period) Cycle Execution

* Update **SCORE: MERIT EMIT & GUBA MERIT** with period performance levels.
* Review **REF: MERIT** and **REF: GUBA MERIT** for completeness and consistency.
* Consolidate the outputs from **REF: MERIT** and **REF: GUBA MERIT** into **EMISSIONS RESULTS - Merit** to create two separate proposals: one to reward USDC and another to reward GRAPE.
{% endstep %}

{% step %}
### Governance Participation Execution

* Import/update Member Participation & Eligibility Spreadsheets inputs.
* Review **GOV PARTICIPATION** outputs (eligibility, voting, calls, raffles).
* Confirm **VINE Rewards** reflect governance participation outcomes.
* Confirm **GRAPE** rewards reflect governance participation outcomes.
* Publish **GOVERNMENT PARTICIPATION REWARDS** payout artifact/proposals: one to reward VINE and another to reward GRAPE.
{% endstep %}
{% endstepper %}

## 7. Notes and Controls (Best Practice)

* **Identity integrity:** All totals should map through **GRAPE MEMBERS** to reduce payout errors.
* **Separation of concerns:** Governance participation rewards remain separate from emissions rollups for clearer auditing.
* **Monthly reporting source of truth:** Monthly **RECURRING PAYMENTS** can only come from **REF: PROPOSAL ARCHITECT** and **REF: DISCORD BOOST** sheets (+ any infrastructure and tool cost), and **EMISSIONS RESULTS - Performance** can only come from **REF: PERFORM** and **REF: GUBA** sheets.
* **Transparency:** Reviews are open and verifiable by the QCG and the community before the final payout recommendation occurs. This design helps maintain transparency during the process.
