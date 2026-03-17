# T21\_ VINE Attendance&#x20;

**GRAPE Rewards & Emissions Management System (GREMS)**\
Version 1.0 | Last Updated: 2025-12-30

Rewarding contributions, participation, and impact across the GRAPE DAO

## 1. Purpose

The **ATTENDANCE VINE** spreadsheet tracks member participation in GRAPE DAO calls and converts participation status into **standardized points**. These points are used to produce **monthly VINE participation totals** that can be referenced in the Governance Participation reward pipeline.

This spreadsheet exists to ensure:

* Consistent scoring across calls
* Transparent review of who participated and how
* A repeatable cycle structure (fixed number of events per cycle)

**Control:** This spreadsheet is an **upstream participation tracker**. It does **not** pay rewards by itself. It supports the **Governance Participation** pipeline by producing auditable attendance/participation totals that are used in downstream reward derivations and payout artifacts.

Figure 1. ATTENDANCE VINE monthly tab example (TAB:APR2025)

## 2. Participation Status Codes (Scoring Rules)

Each call records one participation status per member:

* **S — Speaker on Stage:** **1.5 points**
* **A — Active in Live Chat:** **1.0 points**
  * Must be meaningful participation (not just emojis/greetings)
* **P — Participation token only:** **0 points**

**Control:** Status assignment is the human-reviewed input. Points are formula-derived and should not be edited manually.

## 3. Monthly Tab Structure (9-Call Cycle Model)

Each monthly tab is labeled by month (e.g., **TAB:APR2025**, **TAB:MAY2025**) but follows a **fixed-cycle rule**:

* Each tab contains **9 DAO calls** (a fixed number of events)
* Tabs are **not strict calendar months**
* A tab may span dates that start/end in neighboring months

Examples:

* **TAB:APR2025** might start on **May 20**
* **TAB:MAY2025** might end on **June 29**

This design keeps reward cycles consistent by anchoring them to **9 events**, not to month boundaries.

Figure 2. Monthly tab example (TAB:MAY2025)

## 4. Participation Tracking Within Each Monthly Tab

Within each monthly tab:

1. Each member receives a status (**S / A / P**) for each of the 9 calls
2. Points are calculated automatically using formulas linked to status
3. Monthly totals (per member) are computed for that 9-call cycle

**Control:** Do not overwrite formulas. If a correction is needed, update the **status** (S/A/P), not the numeric outputs.

## 5. Master Consolidation Tab: TAB:2025 VINE MONTHLYS

The tab **TAB:2025 VINE MONTHLYS** consolidates totals from each monthly tab into a single annual view.

Each month/cycle is represented as:

* A clearly labeled **Start Date** and **End Date** (the reward window)
* A **pair of columns** per cycle:
  * **Name**
  * **Score** (the calculated total points)

This enables:

* Side-by-side cycle comparison
* Easy extraction for downstream reporting/proposal prep
* A consistent audit trail across the year

Figure 3. Annual rollup example (TAB:2025 VINE MONTHLYS)

## 6. GIST Tab Workflow (Monthly Reward Proposal Export)

To prepare a clean, public-facing proposal artifact, follow the workflow below.

{% stepper %}
{% step %}
### Create a new GIST tab for the cycle

* Example tab name: **TAB:2025 GIST-PDF: May20–Jun29**
* Pull in the relevant **Name + Score** range from **TAB:2025 VINE MONTHLYS**
{% endstep %}

{% step %}
### Reduce clutter

* Hide older GIST tabs after the cycle is completed
{% endstep %}

{% step %}
### Export

* Export the current GIST tab as **PDF** for public posting / proposal presentation

**Tip (Required):** Before exporting, spot-check that each participant’s **S/A/P** status is accurate for all 9 calls—this is the highest-impact source of errors.
{% endstep %}
{% endstepper %}

Figure 4. GIST export tab example (cycle-range view)

## 7. Controls and Best Practices

* **Cycle integrity:** Each “month” tab must contain **exactly 9 calls** (do not add/remove events mid-cycle).
* **Status-first corrections:** Fix errors by changing **S/A/P**, not by editing point totals.
* **Transparency:** Keep the cycle date range visible in the master tab and the GIST export.
* **No manual overrides:** Proposal-facing totals must match the exported GIST values exactly.
* **Separation of concerns:** Attendance/VINE participation tracking remains part of the **Governance Participation** track and should not be merged into Performance/Merit emissions rollups.
