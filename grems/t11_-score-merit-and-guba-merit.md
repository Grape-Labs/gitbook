# T11\_ SCORE MERIT & GUBA MERIT

**GRAPE Rewards & Emissions Management System (GREMS)**\
Version 1.0 | Last Updated: 2025-12-30

Rewarding contributions, participation, and impact across the GRAPE DAO

### **1. Purpose**

The **SCORE: MERIT EMIT & GUBA MERIT** tab is the **Merit-period calculation engine** in GREMS. It converts the _community-decided Merit scores_ (captured upstream in **SCORE: MERIT SCORING**) into:

* **MERIT EMIT (GRAPE)** — payout calculations for Merit GRAPE rewards
* **GUBA MERIT (USDC)** — payout calculations for Merit USDC rewards

This tab is designed to provide a transparent, formula-driven bridge between **Merit scoring decisions** and the **payout-ready REF sheets** used for proposals and reporting.

**Control:** This tab **does not collect scoring decisions**. It **only calculates** emissions from data sourced upstream.

### **2. Role in GREMS Data Flow**

**Budget Pools + GRAPE MEMBERS**\
→ **SCORE: MERIT SCORING** _(community inputs / scoring decisions)_\
→ **SCORE: MERIT EMIT & GUBA MERIT** _(this tab: converts scoring into GRAPE + USDC)_\
→ **REF: MERIT** _(GRAPE totals by member)_\
→ **REF: GUBA MERIT** _(USDC totals by member)_\
→ **Emissions Results Interface – Merit** _(period rollup reporting)_

### **3. Source of Truth for Scoring Data**

There are **no dropdown menus** on this sheet.

All member scoring rows (Discord Name, Merit Type, Performance Level / weight level) are **pulled from**:

* **SCORE: MERIT SCORING** (the sheet where scoring decisions are entered and reviewed)

This means the operational rule is:

* **Edit scoring only in SCORE: MERIT SCORING**
* **Do not hand-edit the member scoring rows in this tab**

### **4. What Gets Updated in This Tab**

This sheet is primarily formula-driven, but it may include limited **operator-maintained parameters** (per the on-sheet instruction “ONLY UPDATE CELLS WITH YELLOW or GREEN BACKGROUND”).

Typical update areas (when present in the template) include:

* **Weight level % mapping** (Level 0–3 → 0% / 33% / 66% / 100%)
* **Max reward per user** (cap rule for the Merit period)
* **Period scaling / quarterly handling** (if the template includes Quarterly Total fields)
* **Pool allocation constants** tied to the period’s approved budgets

**Control:** If a value changes DAO-wide (caps, level ratios), update it **once** in the designated parameter cells—never inside calculated columns.

### **5. Sheet Sections (How to Read It)**

#### **5.1 Imported Scoring Rows (Top Input Block)**

This block is a **read-only view** of scored entries coming from **SCORE: MERIT SCORING**, typically showing:

* Discord Name
* Merit Type (category/subcategory)
* Performance/weight level
* Calculated outputs (GRAPE and/or USDC totals per row)

&#x20;**Figure 1.** _SCORE: MERIT EMIT & GUBA MERIT — Imported Scoring Rows (from SCORE: MERIT SCORING) and Row-Level Totals (GRAPE + USDC)_

#### **5.2 MERIT EMIT Block (GRAPE Calculation)**

This section calculates **GRAPE** Merit emissions from the imported scoring rows, applying:

* Weight level percentages
* Task/category pool allocations
* Candidate counts (active members with non-zero weights)
* Max reward per user (cap), where configured
* Period totals (and “Quarterly Total” if the period is multi-month)

Outputs here are the basis for downstream **REF: MERIT** totals.

**Figure 2.** _MERIT EMIT (GRAPE) — Merit Period Calculation Block (weights, candidate counts, task pool allocation, totals)_

#### **5.3 GUBA MERIT Block (USDC Calculation)**

This section calculates **USDC** Merit emissions using the same structure, but against the **GUBA MERIT** pools and constraints.

Outputs here are the basis for downstream **REF: GUBA MERIT** totals.

**Figure 3.** _GUBA MERIT (USDC) — Merit Period Calculation Block (weights, candidate counts, task pool allocation, totals)_

####

#### **5.4 Category Summary / Totals (Right-Side Summary)**

This block provides rollups (typically by Merit Type) so reviewers can sanity-check:

* Total distributed vs pool
* Remaining amounts (if tracked)
* Average weight behavior
* Distribution by category (helps detect scoring anomalies)

**Figure 4.** _Merit Type Summary — Category Rollup Table (SUM of Perform, SUM of Remaining, AVERAGE of Perform)_

### **6. Controls and Best Practices**

* **No scoring inputs here:** all scoring is done upstream in **SCORE: MERIT SCORING**.
* **Do not overwrite formulas:** only adjust designated parameter cells (yellow/green).
* **Identity integrity:** all member identity mapping must reconcile through **GRAPE MEMBERS** downstream.
* **Merit does not include SOL in GREMS design:** this sheet’s Merit outputs are **GRAPE + USDC**.
* **Auditability:** this sheet should be reproducible solely from (a) approved budgets + (b) SCORE: MERIT SCORING inputs.

### **7. Recommended Merit Run Order (Operator View)**

{% stepper %}
{% step %}
### Confirm Merit period budgets are approved (Budget Pools)

Ensure the Merit period budgets are approved before proceeding.
{% endstep %}

{% step %}
### Confirm GRAPE MEMBERS is current

Verify the GRAPE MEMBERS list is up-to-date.
{% endstep %}

{% step %}
### Enter / finalize scoring decisions in SCORE: MERIT SCORING during open review

Complete all scoring in SCORE: MERIT SCORING; do not enter scores in this tab.
{% endstep %}

{% step %}
### Verify this tab updates correctly

* MERIT EMIT totals (GRAPE)
* GUBA MERIT totals (USDC)
* Any caps / candidate counts / totals
{% endstep %}

{% step %}
### Confirm downstream payout tabs

* REF: MERIT
* REF: GUBA MERIT
{% endstep %}

{% step %}
### Confirm results in the Emissions Results Interface – Merit rollup/report

Check the final rollup/report for the period in the Emissions Results Interface – Merit.
{% endstep %}
{% endstepper %}
