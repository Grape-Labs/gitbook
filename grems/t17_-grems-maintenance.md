# T17\_ GREMS Maintenance

**GRAPE Rewards & Emissions Management System (GREMS)**\
Version 1.0 | Last Updated: 2025-12-30

Rewarding contributions, participation, and impact across the GRAPE DAO

## 1. Purpose

This document explains how to maintain the **GREMS “maintenance / ops” tabs** that support **monthly operational payments and auxiliary reward streams** that are **not part of Performance or Merit emissions or Governance Participation rollups.**

These tabs feed the **RECURRING PAYMENTS** artifact/proposal (monthly payment list) and help ensure monthly payments remain accurate, auditable, and easy to reproduce.

## 2. Naming Standard

To avoid confusion and keep documentation reusable:

* **In documentation:** use 20XX (evergreen template)
* **In the live workbook:** use the real year (2025, 2026, etc.)

Examples:

* REF: 20XX DISCORD BOOST (example: REF: 2025 DISCORD BOOST)
* REF: 20XX RECURRING PAYMENTS (example: REF: 2025 RECURRING PAYMENTS)

## 3. Scope and Controls

### 3.1 What this guide covers

These tabs support **Recurring Payments** and auxiliary rewards:

| Tab                              | What it tracks                                                                       | Output relevance in GREMS                                |
| -------------------------------- | ------------------------------------------------------------------------------------ | -------------------------------------------------------- |
| **REF: PROPOSAL ARCHITECT**      | SOL “rent” reimbursement + bonus for proposal authors                                | Feeds **RECURRING PAYMENTS** (SOL)                       |
| **REF: 20XX DISCORD BOOST**      | USDC rewards for Discord boosters                                                    | Feeds **RECURRING PAYMENTS** (USDC)                      |
| **REF: 20XX RECURRING PAYMENTS** | Monthly operational payments (infra/tools/subscriptions + auto-linked reward totals) | Becomes the **RECURRING PAYMENTS** payment list proposal |

### 3.2 Separation-of-artifacts (GREMS Control)

* **RECURRING PAYMENTS** is **separate** from **Emissions Results – Performance** and **Emissions Results – Merit** rollups.
* **RECURRING PAYMENTS** can only consolidate from:
  * REF: PROPOSAL ARCHITECT (SOL)
  * REF: DISCORD BOOST (USDC)
  * approved **infrastructure/tool costs** (USDC and/or SOL)
* It must **not** be merged into Performance and Merit emissions, or Governance Participation Rewards reporting/proposals.

## 4. TAB: REF: PROPOSAL ARCHITECT (Maint. + Payout Basis)

### 4.1 Purpose

REF: PROPOSAL ARCHITECT is the **master log of governance proposal activity**, capturing proposal metadata and calculating **SOL rent reimbursement** (and bonus SOL) for proposal architects.

### 4.2 Source (where data comes from)

Proposal information is copied from the DAO’s governance portal (e.g., governance.so / Realms).

### 4.3 Constants (Cost Parameters)

These values drive the SOL reimbursement calculation and can be updated DAO-wide when parameters change.

| Cell   | Parameter                           | Meaning                                 | Example value (SOL) |
| ------ | ----------------------------------- | --------------------------------------- | ------------------- |
| **B2** | Proposal Creation Cost              | SOL cost per proposal created           | 0.00543576          |
| **B3** | Token Transfer Instruction Cost     | SOL cost per token transfer instruction | 0.00238236          |
| **B4** | Grant Voting Power Instruction Cost | SOL cost per grant instruction          | 0.00402984          |
| **B5** | Bonus SOL                           | Bonus applied later per proposal        | 0.05                |

### 4.4 Proposal Log Columns (A–J)

| Column | Field                           | Description                            |
| ------ | ------------------------------- | -------------------------------------- |
| A      | Proposal Title                  | Title/label of the proposal            |
| B      | Signed Off At                   | Timestamp the proposal was signed off  |
| C      | Ends At                         | End timestamp (used to group by month) |
| D      | Proposal URL                    | Direct link to proposal page           |
| E      | Wallet                          | Proposal author wallet                 |
| F      | Discord Name                    | Author attribution                     |
| G      | Proposal Creation Count         | Typically “1” per proposal row         |
| H      | Token Transfer Instructions     | Count visible in proposal details      |
| I      | Grant Voting Power Instructions | Count visible in proposal details      |
| J      | Proposal Rent (SOL)             | Calculated SOL reimbursement           |

### 4.5 Core Formula (Column J)

Proposal Rent (SOL) is calculated as:

| Field               | Formula                           |
| ------------------- | --------------------------------- |
| Proposal Rent (SOL) | =(Gx\*$B$2)+(Hx\*$B$3)+(Ix\*$B$4) |

### 4.6 Monthly Wallet Summary (N–U)

This section aggregates totals per wallet/Discord for the selected month.

| Column | Output                     | What it does                             |
| ------ | -------------------------- | ---------------------------------------- |
| N      | Unique Wallets             | Lists unique wallets for the month       |
| O      | Discord Names              | Maps wallet → Discord name               |
| P      | Base Rent (SOL)            | Sum of Column J per wallet               |
| S      | Proposal Count             | Sum of proposals created                 |
| Q      | Total + Bonus (SOL)        | Base rent + (Proposal Count × Bonus SOL) |
| R      | Bonus Only (SOL)           | Total+Bonus minus Base                   |
| T      | Transfer Instruction Total | Total token transfer instructions        |
| U      | Grant Instruction Total    | Total grant voting power instructions    |

Maintenance note (important): When creating a new month block, copy formulas from a prior month and **update the ranges** so the month summarizes the correct row set. Incorrect ranges = incorrect payouts.

### 4.7 Maintenance Checklist (REF: PROPOSAL ARCHITECT)

* Add new row(s) for each new proposal.
* Copy/paste: Title, dates, URL, wallet, Discord name.
* Count instructions (token transfer + voting power grant).
* Confirm Column J auto-calculates.
* Confirm the monthly summary block totals match expected activity.

## 5. TAB: REF: 20XX DISCORD BOOST (Booster Rewards → Recurring Payments)

### 5.1 Purpose

Tracks Discord server boosts per member and calculates USDC rewards based on a fixed per-boost rate (example: **$5 USDC / boost**).

### 5.2 Reward Rate

| Cell (example) | Parameter         | Meaning                          |
| -------------- | ----------------- | -------------------------------- |
| C3             | Boost Reward Rate | USDC paid per boost (e.g., 5.00) |

### 5.3 Core Columns (Identity)

| Column | Field          | Description              |
| ------ | -------------- | ------------------------ |
| A      | Discord Name   | Booster’s Discord handle |
| B      | Discord ID     | Booster’s Discord ID     |
| C      | Wallet Address | Payment wallet           |

### 5.4 Monthly Tracking Layout (Jan–Dec)

Each month is a **two-column pair**:

| Month Pair | First Column | Second Column |
| ---------- | ------------ | ------------- |
| JAN        | # Boosts     | Reward (USDC) |
| FEB        | # Boosts     | Reward (USDC) |
| …          | …            | …             |
| DEC        | # Boosts     | Reward (USDC) |

### 5.5 Reward Formula

| Field                 | Formula                    |
| --------------------- | -------------------------- |
| Monthly Reward (USDC) | =Boosts \* $RewardRateCell |

### 5.6 Boost Logging Procedure

1. Open Discord → GRAPE server → **Server Settings** → **Server Boost Status**.
2. Record/update each booster’s name/ID/wallet.
3. Enter the **# of boosts** for the current month.
4. Confirm reward cells auto-calculate.

### 5.7 Output Use

Monthly totals from this tab are pulled into the **RECURRING PAYMENTS** payment list as the USDC component of “Discord Boost Rewards.”

## 6. TAB: REF: 20XX RECURRING PAYMENTS (Monthly Payment List)

### 6.1 Purpose

This tab is the **monthly ledger** for operational expenses and auxiliary rewards paid by the DAO. It becomes the payout-ready structure for the **RECURRING PAYMENTS** proposal.

### 6.2 Core Layout

Columns A–B: Payment item + recipient wallet

| Column | Field                    | Description                |
| ------ | ------------------------ | -------------------------- |
| A      | Item/Service Description | What is being paid and why |
| B      | Wallet Address           | Recipient wallet           |

Columns C–Z (and beyond): Monthly currency pairs\
Each month is a two-column pair: **USDC** then **SOL**.

| **Month**    | **USDC Column** | **SOL Column** |
| ------------ | --------------- | -------------- |
| January      | C               | D              |
| February     | E               | F              |
| March        | G               | H              |
| ..and so on. | <p><br></p>     | <p><br></p>    |

### 6.3 Common Payment Rows (Examples)

These are typical recurring items (the list may evolve based on DAO needs):

| Item / Category                  | Example type            | Entry method                                 |
| -------------------------------- | ----------------------- | -------------------------------------------- |
| RPC Infrastructure (e.g., Shyft) | Infra/tool cost         | Manual                                       |
| Discourse subscription           | Service                 | Manual                                       |
| RenderForest (yearly)            | Service                 | Manual (single month)                        |
| DAO Compliance Representative    | Internal role           | Manual                                       |
| Symmetry Challenge Basket        | Program cost            | Manual                                       |
| Discord Boost Rewards            | Auxiliary reward stream | **Auto-linked** from REF: 20XX DISCORD BOOST |
| Proposal Architect Creation      | Auxiliary reward stream | **Auto-linked** from REF: PROPOSAL ARCHITECT |

### 6.4 Totals Row (Monthly Totals)

A totals row contains SUM formulas to compute total USDC and SOL per month. These totals are often used in proposal summaries for transparency.

### 6.5 Key Control: Don’t overwrite formulas

Auto-linked rows must remain formula-driven (Discord Boost + Proposal Architect). Manual edits in those cells can silently break monthly totals.

### 6.6 Monthly Maintenance Checklist (RECURRING PAYMENTS)

* Verify each ongoing service is still active and priced correctly.
* Confirm wallet addresses for vendors/recipients.
* Confirm Discord boost totals pulled correctly for the month.
* Confirm proposal architect totals pulled correctly for the month.
* Confirm totals row matches the visible line items.

## 7. Recommended Update Cadence

| Frequency                             | What to update               | Why                                              |
| ------------------------------------- | ---------------------------- | ------------------------------------------------ |
| Weekly / as proposals occur           | REF: PROPOSAL ARCHITECT      | Keeps SOL reimbursements current                 |
| Monthly (start of month)              | REF: 20XX DISCORD BOOST      | Captures active boosters accurately              |
| Monthly (before proposal publication) | REF: 20XX RECURRING PAYMENTS | Ensures payment list is correct before execution |

## 8. Year Transition Procedure (End-of-Year)

1. Duplicate year-based sheets (e.g., REF: 2025 DISCORD BOOST → REF: 2026 DISCORD BOOST).
2. Duplicate REF: 2025 RECURRING PAYMENTS → REF: 2026 RECURRING PAYMENTS.
3. Update formula links in the new recurring payments tab to point to the **new year** tabs.
4. Archive old year tabs (keep them read-only for auditability).
5. Validate January totals in the new year before publishing the first proposal.

## 9. Glossary (Quick Reference)

* **RECURRING PAYMENTS:** Monthly payment list artifact/proposal (ops costs + auxiliary rewards).
* **REF: PROPOSAL ARCHITECT:** Proposal activity log → SOL reimbursements/bonuses.
* **REF: 20XX DISCORD BOOST:** Boosters log → USDC booster rewards.
* **Separation-of-artifacts control:** Recurring Payments is **not** merged into emissions rollup reporting/proposals.
