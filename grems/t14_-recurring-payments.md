# T14\_ RECURRING PAYMENTS

{% stepper %}
{% step %}
### Purpose

The **REF: RECURRING PAYMENTS** tab is the DAO’s **monthly payment list** for operational expenses and recurring reward streams. It aggregates **USDC and SOL** amounts that are paid on a recurring cadence and supports monthly proposal creation by providing a clear, auditable breakdown of:

* What is being paid (service/tool/role)
* Who is being paid (recipient wallet)
* How much is being paid each month (USDC + SOL)
* Monthly totals used in proposal summaries (GIST / public reporting)

**Control:** This tab is a **Recurring Payments artifact**. It is separate from the **Emissions Results Interface** rollups and does not include performance/merit emissions totals.
{% endstep %}

{% step %}
### What This Tab Includes

Recurring payments typically include:

* **Operational tools / infrastructure** (subscriptions or services)
* **Standing reimbursements or fixed operating costs** (where approved)
* **Monthly add-on reward streams** that are paid outside emissions rollups, such as:
  * **Discord Boost rewards**
  * **Proposal Architect rent/refunds + rewards**
{% endstep %}

{% step %}
### Tab Layout and Structure

#### Columns A–B: Payment Entries and Recipient Wallets

|            |                            |                                                                                     |
| ---------- | -------------------------- | ----------------------------------------------------------------------------------- |
| **Column** | **Field**                  | **Meaning**                                                                         |
| A          | Item / Service Description | A short label describing what is being paid (example: “RPC Infrastructure – Shyft”) |
| B          | Wallet                     | Recipient wallet address for the payment                                            |

#### Standard Recurring Line Items (Rows A7+)

The tab maintains a consistent set of recurring rows. Typical entries include:

* RPC Infrastructure – Shyft
* Discord Boost Rewards for Contributors
* Discourse – Standard Monthly Plan
* Render Forest – Yearly Plan (Nov 2024 – Nov 2025) _(recorded in the month(s) it is paid or allocated, per DAO practice)_
* DAO Compliance Representative _(if applicable)_
* Symmetry Challenge Basket _(if applicable)_
* Proposal Architect Creation

**Figure 1.** _REF: 2025 RECURRING PAYMENTS (Monthly + Yearly payment grid, USDC/SOL)_
{% endstep %}

{% step %}
### Monthly Payment Columns (USDC + SOL)

Each month is represented by **two adjacent columns**:

* **USDC** amount for the month
* **SOL** amount for the month

Example mapping (typical pattern):

* **January** = Columns **C–D**
* **February** = Columns **E–F**
* …continuing through the year in repeating USDC/SOL pairs.

**Figure 2.** _REF: 2025 RECURRING PAYMENTS — Example Month View (January-February)_
{% endstep %}

{% step %}
### Data Entry and Update Method

Recurring payments are populated using **two methods**, depending on the item type:

#### Manual Entries (Fixed Costs / Known Amounts)

Some payments are entered directly because they are fixed, invoice-based, or otherwise known:

* RPC Infrastructure – Shyft
* Discourse – Standard Monthly Plan
* Render Forest – Yearly Plan (Nov 2024 – Nov 2025)
* DAO Compliance Representative (if applicable)
* Symmetry Challenge Basket (if applicable)

**Operator rule:** Enter the correct **wallet (Column B)** and the month amount in the appropriate **USDC/SOL month columns**.

#### Auto-Imported Entries (Linked Reward Streams)

Other rows are formula-linked to their source tabs so they update automatically:

* **Discord Boost Rewards for Contributors** → sourced from **REF: DISCORD BOOST**
* **Proposal Architect Creation** → sourced from **REF: PROPOSAL ARCHITECT**

**Control:** Do not overwrite cells that contain import formulas.
{% endstep %}

{% step %}
### Monthly Totals (Row Totals for Proposals)

A totals row (commonly near the bottom, e.g., “TOTAL USDC & Solana”) contains **SUM formulas** for each month that calculate:

* Total monthly **USDC** spend
* Total monthly **SOL** spend

These totals are commonly copied into proposal summaries (GIST tables) for transparent reporting.
{% endstep %}

{% step %}
### Operating Controls and Review Checklist

Before publishing a Recurring Payments proposal:

* Confirm all **manual line items** have the correct wallet and correct month amounts.
* Confirm **auto-imported rows** (Discord Boost + Proposal Architect) are syncing and populating correctly.
* Confirm monthly **USDC/SOL totals** match expectations for the cycle.
* Ensure this tab is used only for **Recurring Payments** and is not mixed with emissions rollup reporting.
{% endstep %}
{% endstepper %}
