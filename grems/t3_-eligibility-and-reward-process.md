# T3\_ Eligibility & Reward Process

**GRAPE Rewards & Emissions Management System (GREMS)**\
Version 1.0 | Last Updated: 2025-12-30

Rewarding contributions, participation, and impact across the GRAPE DAO

## 1. Purpose

This document explains, in plain language, how GRAPE DAO members become eligible for rewards and how rewards are calculated, reviewed, and prepared for distribution using **GREMS**. To illustrate the process, let's consider a hypothetical member named Alex. Alex joins GRAPE DAO by attending community calls and engaging in governance activities. By completing tasks such as providing feedback on proposals, attending calls, participating in raffles, and joining discussions, Alex earns recognition and participation points. The points are then recorded, reviewed, and validated during monthly and quarterly community meetings. There are always community members tasked with keeping track. Once validated, Alex's contributions are entered into the GREMS spreadsheet, where calculations are performed to determine his reward payout. Finally, Alex receives his rewards in GRAPE, USDC, or SOL tokens directly to his wallet, following the community's approval of the emission proposal.

GREMS supports three parallel reward pipelines:

1. **Performance Rewards (Monthly)**
2. **Merit Rewards (Period-Based)**
3. **Governance Participation Rewards (Standalone Payout Artifact)**

## 2. Who Can Earn Rewards?

Any GRAPE DAO community member **in good standing** may earn rewards. Being in good standing means a member has no bans, participates in calls and raffles, and shows a willingness to volunteer for tasks.

You do not need anything to start. It all begins by attending calls and listening for opportunities to join a project or a specific recurring task. You can start by:

* Participating in GRAPE DAO governance (calls/meetings, voting, raffles)
* Contributing through tasks, bounties, or recurring roles
* Building a track record that may be recognized in merit cycles

### 2.1 Eligibility (High-Level)

Eligibility requirements can vary by reward type and are defined by the DAO. In general, eligibility is determined through:

* **Member status and baseline participation requirements**
* **Governance participation signals** (attendance, voting, eligibility status, raffles)
* **Contribution validation** (tasks completed and reviewed)

**Important:** Governance participation eligibility is processed in a dedicated, standalone pipeline and does **not** roll into emissions rollup reporting. This separation ensures members can receive rewards for the simplest tasks, such as attending calls, voting, boosting the Discord server, or other passive activities that don’t require more active engagement.

## 3. Ways You Can Earn Rewards (Three Pipelines)

### 3.1 Governance Participation Rewards (Standalone)

Governance Participation rewards recognize engagement signals such as:

* **Attendance at meetings/calls**
* **Voting on proposals** (where applicable)
* **Eligibility to earn rewards** (baseline requirements defined by the DAO)
* **Participating in raffles** (DAO-defined rules)

What you can earn

* **VINE** (derived from governance participation outcomes)
* Possible additional prizes (NFTs or tokens such as GRAPE, JUP, BONK, etc., when applicable)

Key rules

* **VINE resets annually.**
* Governance Participation rewards are distributed via a **standalone payout artifact** and **do not roll into EMISSIONS RESULTS** reporting.

### 3.2 Performance Rewards (Monthly)

Performance rewards recognize monthly contributions across approved categories (recurring tasks, ongoing roles, and monthly work outputs).

What you can earn

* **GRAPE / USDC / SOL** (varies by category rules and budget configuration)

Typical examples of monthly performance work

* Documentation, research, moderation, operations support, proposal support, product testing, metrics, communications, and other approved categories.

### 3.3 Merit Rewards (Period-Based)

Merit rewards recognize period-based excellence, impact, consistency, growth, and exceptional contributions.

What you can earn (in this system design)

* **GRAPE + USDC**

Note: Some merit category names may reference SOL or other assets (e.g., “Treasury Growth”), but merit emissions in this design are calculated and tabulated as **GRAPE and USDC**.

## 4. What Gets Paid and Where It Comes From

### 4.1 Summary Table (Beginner View)

| **Reward Path**                           | **Details**                                                                                                                                                                                                                                                                             |
| ----------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Governance Participation (Standalone)** | <p><strong>DO:</strong> Attendance, voting, eligibility, raffles</p><p><br></p><p><strong>EARN</strong>: VINE + GRAPE + possible other prizes</p><p><br></p><p><strong>OUTPUT: GOVERNMENT PARTICIPATION REWARDS</strong></p><p><em>(standalone; not part of emissions rollups)</em></p> |
| **Performance (Monthly)**                 | <p><strong>DO:</strong> Monthly tasks or other contributions</p><p><br></p><p><strong>EARN:</strong> GRAPE / USDC / SOL</p><p><br></p><p><strong>OUTPUT-1: RECURRING PAYMENTS</strong><br><strong>OUTPUT-2: EMISSIONS RESULTS - Performance</strong></p>                                |
| **Merit (Period)**                        | <p><strong>DO:</strong> Period-based excellence &#x26; high-impact on tasks performed</p><p><br></p><p><strong>EARN:</strong> GRAPE + USDC</p><p><br></p><p><strong>OUTPUT: EMISSIONS RESULTS - Merit</strong></p><p><em>(from REF: MERIT + REF: GUBA MERIT)</em></p>                   |

## 5. How Rewards Are Verified (Open Review)

Rewards are validated through a transparent review process involving:

* The **DAO Quality Control Group (QCG)**
* The **community**, through open review during regular meetings/calls

Typical review includes:

* Confirming participation inputs (for governance participation)
* Confirming work completion and performance levels (monthly performance)
* Confirming merit period scoring and recognition (period merit)

Reviews are designed to be open and auditable before final payout artifacts are published.

## 6. The GREMS Spreadsheet System (User-Friendly Map)

GREMS is implemented via interconnected spreadsheets that:

* Track budgets and enforce caps
* Record performance and participation outcomes
* Map totals to the canonical member roster (**GRAPE MEMBERS**)
* Produce payout-ready artifacts and rollup reports.

### 6.1 Upstream Inputs (Common Foundations)

* **Budget Pools per Category**\
  Approved budgets per category for a cycle (monthly or merit period).
* **GRAPE MEMBERS (Roster / Identifiers)**\
  Canonical identity mapping for payout accuracy.
* **Member Participation & Eligibility Spreadsheets**\
  Governance participation inputs (attendance, voting, eligibility, raffles).

These do **not** feed VINE Rewards directly; they feed **GOV PARTICIPATION**.

## 7. Data Flow (Accurate to GREMS Diagram)

### 7.1 Governance Participation Pipeline (Standalone)

Member Participation & Eligibility Spreadsheets → GOV PARTICIPATION (eligibility/voting/calls/raffles) → VINE Rewards (derived from GOV PARTICIPATION outcomes) → GOVERNMENT PARTICIPATION REWARDS (standalone payout artifact)

**Important control:** Governance Participation does **not** flow into EMISSIONS RESULTS rollups.

### 7.2 Performance Pipeline (Monthly)

Budget Pools + GRAPE MEMBERS → SCORE: PERFORM & GUBA EMIT (calculates GRAPE / USDC / SOL) → REF: PERFORM (GRAPE totals) → REF: GUBA (USDC + SOL totals)

Additional monthly streams:

* REF: PROPOSAL ARCHITECT (SOL refunds + rewards) → RECURRING PAYMENTS
* REF: DISCORD BOOST (USDC rewards) → RECURRING PAYMENTS

Consolidation + reporting:

* REF: PROPOSAL ARCHITECT + REF: DISCORD BOOST → RECURRING PAYMENTS (monthly payment list)

**Source of truth for monthly reporting:** EMISSIONS RESULTS - Performance (Monthly) derives from REF: PERFORM & REF: GUBA sheets.

### 7.3 Merit Pipeline (Period)

Budget Pools + GRAPE MEMBERS → SCORE: MERIT EMIT & GUBA MERIT (calculates GRAPE / USDC) → REF: MERIT (GRAPE totals) → REF: GUBA MERIT (USDC totals) → EMISSIONS RESULTS (Period Rollup Report)

## 8. Step-by-Step: How Rewards Get From Activity to Payout

{% stepper %}
{% step %}
### Governance Participation (Standalone)

* Participation inputs are updated in **Member Participation & Eligibility Spreadsheets.**
* Outputs are processed in **GOV PARTICIPATION.**
* **VINE Rewards** are derived from GOV PARTICIPATION outcomes.
* Results are consolidated into **GOVERNMENT PARTICIPATION REWARDS.**
* Governance participation payouts are distributed via the DAO’s chosen method and tracked separately from emissions rollups. Typical payout methods are direct wallet transfers after the DAO community approves the emission proposal.
{% endstep %}

{% step %}
### Monthly Performance

* Budgets are confirmed for the month (Budget Pools per Category).
* Performance levels are entered into **SCORE: PERFORM** and **GUBA EMIT.**
* Totals appear in **REF: PERFORM** and **REF: GUBA.**
* Add-ons (if applicable) are updated:
  * **REF: PROPOSAL ARCHITECT**
  * **REF: DISCORD BOOST**
* **REF: PROPOSAL ARCHITECT & REF: DISCORD BOOST** monthly totals consolidate into a **RECURRING PAYMENTS** proposal.
* **REF: PERFORM + REF: GUBA** totals consolidate into **EMISSIONS RESULTS – Performance** proposals.
* The DAO executes monthly payouts based on the approved artifacts/proposals.
{% endstep %}

{% step %}
### Merit (Period)

* Budgets are confirmed for the merit period.
* Period performance levels are entered into **SCORE: MERIT EMIT** and **GUBA MERIT.**
* Totals appear in **REF: MERIT** and **REF: GUBA MERIT.**
* Period reporting is produced in **EMISSIONS RESULTS (Period Rollup Report).**
* The DAO executes merit payouts based on approved artifacts/proposals.
{% endstep %}
{% endstepper %}

## 9. Governance Participation Categories (Tracked Signals)

Governance Participation categories are engagement signals used to determine eligibility and participation-based rewards.

**Categories**

* **Attendance at Meetings**
* **Voting on Proposals**
* **Eligibility to Earn Rewards**
* **Participating in Raffles**

**Tracking & consolidation**

* Inputs are recorded in Member Participation & Eligibility spreadsheets.
* Processed in **GOV PARTICIPATION**
* May produce outputs in **VINE Rewards** and **GOVERNMENT PARTICIPATION REWARDS**

## 10. Notes and Controls (Best Practices)

* **Identity integrity:** Rewards should map through **GRAPE MEMBERS** to reduce payout errors.
* **Separation of concerns:** Governance participation rewards remain separate from emissions rollups for clarity and auditing.
* **Budget enforcement:** Budget pools constrain category distributions to prevent overspending.
* **Transparency:** Reviews occur openly before payout artifacts are finalized.

## 11. Glossary (Quick Reference)

* **GREMS:** GRAPE Rewards & Emissions Management System
* **QCG:** Quality Control Group (task validation and review facilitation)
* **VINE:** Governance participation reward token; resets annually
* **SCORE: PERFORM & GUBA EMIT:** Monthly scoring and calculation sheet (GRAPE/USDC/SOL)
* **REF Sheets:** Reference tabs that tabulate final payout totals by member
* **RECURRING PAYMENTS:** Monthly consolidated payout instruction list
* **EMISSIONS RESULTS:** Rollup reports for transparency/accounting (Monthly and Period)
* **GOV PARTICIPATION:** Governance participation processing sheet (eligibility/voting/calls/raffles)
* **GOVERNMENT PARTICIPATION REWARDS:** Standalone governance payout artifact

## 12. Contact / Where to Start

* Join the community: [https://discord.com/invite/grapedao](https://discord.com/invite/grapedao)
* Attend calls/meetings: \[Visit the Events channel at Discord for times and schedule]
* Volunteer for tasks: \[During calls, announce it on the Live Chat channel. Say you have time to take one or more tasks, or want to help out. This announcement lets everyone know you are available for tasks.]
* Ask questions during open review: \[Live Chat Meeting or DAO Discord channels]
