# T12\_ PROPOSAL ARCHITECT

**GRAPE Rewards & Emissions Management System (GREMS)**\
Version 1.0 | Last Updated: 2025-12-30

Rewarding contributions, participation, and impact across the GRAPE DAO

## _**1. Purpose**_

_The_ _**REF: PROPOSAL ARCHITECT**_ _tab is the_ _**master log**_ _for tracking proposal-architect activity and calculating_ _**SOL “rent” rewards**_ _earned through proposal creation and execution work. It captures proposal metadata from_ _**governance.so**, calculates a per-proposal SOL amount, and produces_ _**monthly wallet totals**_ _used to build the_ _**RECURRING PAYMENTS**_ _payout proposal._

_**It enables reviewers and operators to:**_

* _Maintain a transparent, auditable record of DAO proposals_
* _Attribute proposal work to_ _**wallets + Discord names**_
* _Calculate SOL rent consistently using DAO-defined constants_
* _Produce monthly totals for payout proposals_

## _**2. Scope and Controls**_

_**Control: Separation from emissions rollups**_\
&#xNAN;_&#x52;EF: PROPOSAL ARCHITECT is a_ _**Recurring Payments source**_. It\* _**does not**_ _roll into the_ _**Emissions Results Interface**_ _and_ _**does not**_ _affect Performance/Merit emissions rollup reporting._

_**Downstream usage**_

* _**REF: PROPOSAL ARCHITECT → RECURRING PAYMENTS**_ _(SOL refunds + rewards)_

## _**3. Inputs (Source of Truth)**_

_All proposal records in Columns_ _**A–I**_ _are sourced from_ [_**governance.so**_](http://governance.so/) _proposal pages:_

_**Required fields captured per proposal**_

* _Proposal title_
* _Signed-off timestamp_
* _End timestamp (used for month grouping)_
* _Proposal URL_
* _Author wallet_
* _Discord name attribution_
* _Instruction counts (Token Transfer + Grant Voting Power)_

_**Figure 1:**_ _Where to copy proposal fields for Columns A–I (governance.so proposal details)_

## _**4. Tab Layout and Column Definitions**_

### _**4.1 Constants (Cells A2–B5)**_

_This block defines the_ _**unit costs**_ _used to compute “rent” in SOL._

|            |                                       |                                                                |
| ---------- | ------------------------------------- | -------------------------------------------------------------- |
| _**Cell**_ | _**Parameter**_                       | _**Meaning**_                                                  |
| _**B2**_   | _Proposal Creation Cost_              | _SOL cost per proposal created_                                |
| _**B3**_   | _Token Transfer Instruction Cost_     | _SOL cost per token-transfer instruction_                      |
| _**B4**_   | _Grant Voting Power Instruction Cost_ | _SOL cost per grant-voting-power instruction_                  |
| _**B5**_   | _Bonus SOL_                           | _Bonus multiplier applied later in the monthly totals section_ |

_**Control:**_ _These constants are DAO-wide reward parameters. Update only when the DAO updates reward rules._

### _**4.2 Proposal Log (Columns A–J)**_

_**Figure 2:**_ _REF: PROPOSAL ARCHITECT tab after data is entered (proposal log view)_

_This is the main, row-by-row proposal ledger._

|              |                                   |                                                                                                            |
| ------------ | --------------------------------- | ---------------------------------------------------------------------------------------------------------- |
| _**Column**_ | _**Field**_                       | _**Meaning / How to Populate**_                                                                            |
| _**A**_      | _Proposal Title_                  | _Copy from governance.so proposal page_                                                                    |
| _**B**_      | _Signed Off At_                   | _Copy from_ [_governance.so_](http://governance.so/)                                                       |
| _**C**_      | _Ends At_                         | _Copy from_ [_governance.so_](http://governance.so/) _(determines which month the proposal is counted in)_ |
| _**D**_      | _Proposal URL_                    | _Copy URL_                                                                                                 |
| _**E**_      | _Wallet_                          | _Author wallet address_                                                                                    |
| _**F**_      | _Discord Name_                    | _Contributor attribution_                                                                                  |
| _**G**_      | _Proposal Creation Count_         | _Typically_ _**1**_ _per proposal row_                                                                     |
| _**H**_      | _Token Transfer Instructions_     | _Count shown in proposal “Instructions” list_                                                              |
| _**I**_      | _Grant Voting Power Instructions_ | _Count shown in proposal “Instructions” list_                                                              |
| _**J**_      | _Proposal Rent in SOL_            | _Auto-calculated rent value_                                                                               |

_**Column J formula**_

* _=(G8\*$B$2)+(H8\*$B$3)+(I8\*$B$4)_

_This computes_ _**base rent**_ _only. Bonus SOL is applied in the monthly totals summary._

### _**4.3 Monthly Wallet Summary - RENT (Columns N–U)**_

_This section aggregates proposal rent_ _**per wallet / per month**_.

**Figure 3:** “Proposal Rent TOTALS” monthly summary output (wallet totals)

|              |                    |                                                     |
| ------------ | ------------------ | --------------------------------------------------- |
| _**Column**_ | _**Function**_     | _**Meaning**_                                       |
| _**N**_      | _UNIQUE()_         | _Unique wallets for the month’s proposal block_     |
| _**O**_      | _UNIQUE()_         | _Discord names corresponding to wallets_            |
| _**P**_      | _SUMIF()_          | _Base rent total (sum of Column J for that wallet)_ |
| _**Q**_      | _P + (S \* Bonus)_ | _Base rent + bonus SOL_                             |
| _**R**_      | _Q - P_            | _Bonus portion only_                                |
| _**S**_      | _SUMIF()_          | _Proposal count total_                              |
| _**T**_      | _SUMIF()_          | _Token transfer instruction total_                  |
| _**U**_      | _SUMIF()_          | _Grant voting power instruction total_              |

_**Important maintenance note (range updates)**_\
&#xNAN;_&#x57;hen you add a new month section, copy the formulas from a prior month and_ _**update the referenced row ranges**_ _so the_ _SUMIF()_ _and_ _UNIQUE()_ _logic reflects the correct month block. Incorrect ranges will produce incorrect totals._

_**Figure 4:**_ _Where to find the “Instructions” count (token transfer vs grant voting power)_

## _**5. Monthly Operating Procedure (Run Steps)**_

{% stepper %}
{% step %}
### Open governance.so and list proposals

Open the governance.so DAO page and list proposals for the target month.
{% endstep %}

{% step %}
### Copy core fields for each proposal

For each proposal:

* Copy/paste Columns A–F from the proposal page.
* Set G = 1 (standard).
* Count H (Token Transfer instructions) and I (Grant Voting Power instructions) from the proposal instructions list.
{% endstep %}

{% step %}
### Confirm base rent calculation

Confirm Column J calculates automatically (base rent).
{% endstep %}

{% step %}
### Group proposals by month

Group proposals into the correct month using Ends At (Column C).
{% endstep %}

{% step %}
### Confirm monthly totals populate

In Columns N–U, confirm the monthly totals populate correctly (and ranges are correct).
{% endstep %}

{% step %}
### Use monthly totals for payouts

Use the monthly wallet totals as the source for the RECURRING PAYMENTS proposal build.
{% endstep %}
{% endstepper %}

## _**6. Outputs and Downstream Usage**_

_**Primary output**_

* _Monthly per-wallet SOL totals (Columns_ _**P–Q**_, _plus supporting counts_ _**S–U**_)\*

_**Used for**_

* _Building the_ _**RECURRING PAYMENTS**_ _payout artifact (SOL refunds + rewards)_

_**Not used for**_

* _Emissions Results Interface reporting_
* _Performance/Merit emissions proposals_
* _Governance Participation payouts_

## _**7. Quality Checks (Recommended)**_

* _**Month alignment:**_ _Ensure “Ends At” dates match the intended payout month grouping._
* _**Instruction counts:**_ _Spot-check a few proposals against governance.so to confirm H/I are correct._
* _**Attribution:**_ _Wallet + Discord name should match the author/contributor record._
* _**Totals sanity:**_ _If a month seems unusually high/low, verify no proposals were missed and that formula ranges match the month block._
