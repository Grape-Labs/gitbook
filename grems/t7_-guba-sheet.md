# T7\_ GUBA Sheet

**GRAPE Rewards & Emissions Management System (GREMS)**\
Version 1.0 | Last Updated: 2025-12-30

Rewarding contributions, participation, and impact across the GRAPE DAO

## 1. Purpose

The **REF: GUBA** tab consolidates and finalizes **Performance (Monthly)** rewards that are paid in **USDC + SOL**. It rolls up member-level allocations calculated in **SCORE: PERFORM & GUBA EMIT**, applies per-member caps, and provides validation checks used for reporting and payout preparation.

REF: GUBA aggregates:

* **USDC allocations** from **SCORE: PERFORM & GUBA EMIT (Columns R–X)**
* **SOL allocations** from **SCORE: PERFORM & GUBA EMIT (Columns Z–AF)**

Control: REF: GUBA is part of the **Performance emissions pipeline** and supports **Emissions Results Interface – Performance** reporting. It does **not** include **Recurring Payments** sources (Proposal Architect, Discord Boost, infrastructure/tool costs), and it does **not** include governance participation (VINE).

## 2. Inputs and Dependencies

### 2.1 Upstream Sources

A) **SCORE: PERFORM & GUBA EMIT**

* USDC rewards: **R–X**
* SOL rewards: **Z–AF**

B) **GRAPE MEMBERS (Roster / Identifiers)**

* Discord name → Discord ID → Wallet address mapping (for final payout readiness)

C) **Emissions Results Interface** (reporting + cap parameters)

* Pulls total pool and max-emission parameters used in REF: GUBA summary metrics and capping logic
* Note: the spreadsheet tab may still be labeled “EMISSIONS RESULTS” in-template, but in documentation we refer to it as **Emissions Results Interface** to avoid confusion with system-level artifacts.

## 3. Tab Layout and Data Flow

### 3.1 Columns A–D: Summary Pivot Table (Auto-Populated)

This section is built via a pivot sourced from the SCORE tab and summarizes _uncapped_ totals per member.

* **Column A:** Discord Name
* **Column B:** Discord ID
* **Column C:** Total Allocation (USDC) — sum of USDC from SCORE
* **Column D:** Total Allocation (SOL) — sum of SOL from SCORE

Function: This is the “raw” rollup used by the capped payout table (F–J).

Figure 1: Pivot Summary (Columns A–D)

### 3.2 Columns F–J: Member Details & Final Rewards (Capped Output)

This section ties members to payout identifiers and computes **final capped rewards**.

* **Column F:** Discord Name (dropdown from GRAPE MEMBERS)
* **Column G:** Discord ID (lookup from GRAPE MEMBERS once name is selected)
* **Column H:** Wallet Address (lookup from GRAPE MEMBERS once name is selected)

Final Rewards (USDC) — Column I (cap applied):\
Example formula:\
\=IFERROR(IF(VLOOKUP(F3,A:C,3,FALSE) < $U$4, VLOOKUP(F3,A:C,3,FALSE), $U$4), 0)

Logic:

* Pull member USDC total from Pivot (A–C)
* If it’s below the max cap (**U4**), pay the pivot amount
* Otherwise cap at **U4**
* If lookup fails, return 0

Final Rewards (SOL) — Column J (cap applied):\
Example formula:\
\=IFERROR(IF(VLOOKUP(F3,A:D,4,FALSE) < $V$4, VLOOKUP(F3,A:D,4,FALSE), $V$4), 0)

Logic: same as USDC, but uses SOL totals and SOL cap (**V4**).

Figure 2: Member Details & Final Rewards (Columns F–J)

### 3.3 Columns L–S: Task Pools (USDC + SOL)

This section defines task-level pools used to allocate USDC/SOL across tasks.

* **Column L:** Task List (task label)
* **Column M:** Task ID (unique task identifier)
* **Column O:** Total Pool (USDC) — pool ratio × total USDC pool
  * Example: =S3 \* $U$2
* **Column Q:** Total Pool (SOL) — pool ratio × total SOL pool
  * Example: =$V$2 \* S3
* **Column S:** Pool Ratio (often hidden)
  * These ratios are DAO-defined (governance decision) and drive the USDC/SOL distribution weights.

Control: Pool ratios are “admin-maintained configuration.” They are not member scoring inputs.

Figure 3: Task Pools (Columns L–S)

### 3.4 Columns T–V: Summary Metrics (Totals, Caps, Productivity)

This block provides rollup metrics for both reward types.

Column T: labels

* FINAL REWARDS
* TOTAL POOL
* PRODUCTIVITY
* MAX EMISSION per member
* TOTAL CANDIDATES
* REWARD per CANDIDATE

Column U (USDC):

* **U1:** Final USDC paid: =SUM($I$3:$I)
* **U2:** Total USDC pool (from Emissions Results Interface)
* **U3:** Productivity: =U1/U2
* **U4:** Max USDC emission per member (from Emissions Results Interface)
* **U5:** Candidate count (members receiving USDC)
* **U6:** Avg USDC per candidate: =U2/U5

Column V (SOL):

* **V1:** Final SOL paid: =SUM($J$3:$J)
* **V2:** Total SOL pool (from Emissions Results Interface)
* **V3:** Productivity: =V1/V2
* **V4:** Max SOL emission per member (from Emissions Results Interface)
* **V5:** Candidate count (members receiving SOL)
* **V6:** Avg SOL per candidate: =V2/V5

Important fix to avoid silent errors:\
Make sure candidate counts are filtered on the correct “non-zero reward” column:

* USDC candidates should key off **USDC reward** (Pivot USDC or Final USDC).
* SOL candidates should key off **SOL reward** (Pivot SOL or Final SOL).\
  If you use the same filter column for both, SOL candidate counts can be wrong.

Figure 4: Summary Metrics + Validation (Columns T–Y)

### 3.5 Columns W–Y: Validation Controls (Wallet Integrity Checks)

These checks ensure reward distributions align with the wallet population.

* **Y5:** Wallet count (unique wallets receiving non-zero rewards)
  * Example: =COUNTA(UNIQUE(FILTER(H3:H100, I3:I100 <> 0))) _(USDC version)_
  * For SOL wallet count, filter on **J <> 0** instead of I.
* **W5:** USDC validation ✅/❌
  * Pass if **USDC candidate count (U5)** equals **wallet count (USDC)**
* **X5:** SOL validation ✅/❌
  * Pass if **SOL candidate count (V5)** equals **wallet count (SOL)**

## 4. Operating Procedure (Run Order)

{% stepper %}
{% step %}
### Pre-Run Updates

* Confirm **GRAPE MEMBERS** roster is current (Discord name, Discord ID, wallet address).
* Confirm **Emissions Results Interface** parameters are correct (total pools + max emission caps).
* Confirm SCORE tab inputs have been reviewed/locked for the cycle.
{% endstep %}

{% step %}
### Build Final Rewards

* Refresh the Pivot Table (A–D).
* Use dropdowns in **F** to ensure all active recipients are included (names added as needed).
* Review **Final Rewards (I, J)** for expected caps and totals.
* Verify **productivity**, **candidate counts**, and **wallet validations** (T–Y).
{% endstep %}

{% step %}
### Outputs Used Downstream

* Final per-member payout-ready totals come from:
  * **Column I (USDC)**
  * **Column J (SOL)**
* These totals feed **Emissions Results Interface – Performance** reporting and the USDC/SOL payout proposal preparation for the Performance cycle.
{% endstep %}
{% endstepper %}

## 5. Notes and Controls (Best Practice)

* **Separation of concerns:** REF: GUBA supports **Performance emissions** only; it does not include **Recurring Payments** streams.
* **Caps must be sourced consistently:** max emission values should come from **Emissions Results Interface** (so reporting and capping stay aligned).
* **SOL precision:** If your payout system requires it, keep SOL unrounded until final export (match the sheet’s “no rounding for Solana” convention).

## 6. Flowchart
