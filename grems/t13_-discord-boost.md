# T13\_ DISCORD BOOST

**GRAPE Rewards & Emissions Management System (GREMS)**\
Version 1.0 | Last Updated: 2025-12-30

Rewarding contributions, participation, and impact across the GRAPE DAO

### 1. Purpose

The **REF: 2025 DISCORD BOOST** tab tracks **monthly Discord Server Boost** activity by GRAPE DAO community members. It links each booster to a **Discord ID** and **wallet address**, records the **number of boosts per month**, and automatically calculates **USDC rewards** using a fixed **reward-per-boost** constant (currently **$5.00 per boost**).

This tab exists to produce a **payout-ready reference** for the **RECURRING PAYMENTS** proposal stream (Discord Boost rewards).

Control (separation of artifacts):

* **REF: 2025 DISCORD BOOST → RECURRING PAYMENTS** (monthly payment list).
* It does **not** roll into the **Emissions Results Interface** and does **not** include governance participation (VINE).

### 2. Where This Tab Fits in GREMS

**Data source:** Discord “Server Boost Status” (manual observation).\
**Output usage:** Monthly totals are used to create a **RECURRING PAYMENTS** proposal that pays **USDC boost rewards**.

### 3. Tab Structure

#### 3.1 Booster Identity Columns (A–C)

These columns identify each booster:

* **Column A — Discord Name**
* **Column B — Discord ID**
* **Column C — Wallet Address**

Figure 1 — Full screenshot of REF: 2025 DISCORD BOOST

**Data entry rule:** This identity block is maintained from verified booster records. Wallets should match the canonical roster where possible (GRAPE MEMBERS).

#### 3.2 Monthly Boost Tracking (D–AA)

The sheet tracks boosts across **January–December**. Each month typically uses **two columns**:

* **Month: # of Boosts**
* **Month: Rewards ($)**

Reward calculation:\
Rewards are computed as:\
**Rewards = Boosts × RewardPerBoost**\
Where **RewardPerBoost** is stored as a constant (e.g., **$5.00**) in the sheet.

Figure 2 — Closeup look (Boost Amount, identity columns, and monthly fields)

### 4. Boost Logging Process (Monthly Update)

{% stepper %}
{% step %}
### Step: Open Discord Server Boost Status

In Discord, open:\
**GRAPE Server → Server Settings → Server Boost Status**
{% endstep %}

{% step %}
### Step: Record/update boosters for the current month

Read the list of boosters and record/update for the current month:

* Discord Name
* Discord ID
* Wallet Address
* **Number of boosts** for the month
{% endstep %}

{% step %}
### Step: Confirm reward-per-boost constant

Confirm the reward-per-boost constant is correct (e.g., **$5.00**).
{% endstep %}

{% step %}
### Step: Confirm month’s reward column

Confirm the month’s reward column auto-calculates.
{% endstep %}
{% endstepper %}

Figure 3 — Screenshot from Discord (Server Boost Status source)

**Best practice:** If a booster changes their Discord name, keep the **Discord ID** as the stable identifier and update the display name accordingly.

### 5. GIST Export Workflow (For Proposal Presentation)

To generate the clean “execution-ready” list used for proposal support materials:

{% stepper %}
{% step %}
### Copy month range

Copy the **current month** range containing **Wallet + Rewards**.
{% endstep %}

{% step %}
### Paste into GIST tab

Paste into a dedicated GIST tab for that month (example naming pattern):\
**TAB: 20XX GIST-PDF:**
{% endstep %}

{% step %}
### Paste values only

**Paste values only** (to avoid linking formulas into the GIST export).
{% endstep %}

{% step %}
### Verify totals

Verify totals match expected monthly sum before exporting.
{% endstep %}

{% step %}
### Export PDF

Export the GIST tab as a PDF for the proposal package.
{% endstep %}
{% endstepper %}

Figure 4 — For GIST usage (paste values only instructions)

**Control:** The GIST tab is for presentation/execution formatting only—source-of-truth remains **REF: DISCORD BOOST**.

### 6. Controls and Validation Checks

Use these checks before publishing a RECURRING PAYMENTS proposal:

* **Wallet completeness:** every paid line must have a valid wallet address.
* **Non-zero filter:** ensure only members with non-zero rewards appear in the month’s payout list (unless explicitly required for reporting).
* **Reward constant check:** confirm the boost reward value (e.g., $5.00) has not changed.
* **Discord proof:** retain a screenshot or reference capture of the month’s “Server Boost Status” view for auditability.

### 7. Output Summary

**Result:** This tab produces a month-by-month, wallet-mapped view of Discord boosting rewards so the DAO can publish a **RECURRING PAYMENTS** proposal paying **USDC** to boosters with transparent, auditable support.
