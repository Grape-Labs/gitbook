# T1\_ Quick Start Guide

**GRAPE Rewards & Emissions Management System (GREMS)**\
Version 1.0 | Last Updated: 2025-12-30

Rewarding contributions, participation, and impact across the GRAPE DAO

## **1. Overview**

**Graperators** are GRAPE DAO community members who volunteer for tasks planned and accepted by the GRAPE DAO. They perform key duties that support initiatives across the DAO and community, including documentation, proposal and bounty writing, presentations, research, marketing, and product development.

The **DAO Quality Control Group (QCG)** and the community conduct weekly, monthly, and period-based reviews of completed tasks to confirm earned rewards. These reviews typically take place during regular GRAPE DAO meetings. Participation in these reviews is open to all community members, supporting complete transparency into how rewards are validated.

Together, the **GRIPv2 Emission** and **VINE Rewards** spreadsheets create the operational foundation of GREMS (also referred to as the **Graperators Emissions Pool Management System**). This group of sheets assists the QCG and the community in reviewing completed tasks and confirming earned rewards. The sheets are interlinked and share input data to automatically calculate rewards based on approved budgets, task rules, and cap constraints.

The GRAPE DAO community approves proposals that distribute final rewards, create new projects, vote on changes to existing tasks, and recommend adjustments to rewards when appropriate.

## **2. Getting Started: How Members Earn Rewards**

{% stepper %}
{% step %}
### Performance (Monthly) Rewards

Earned by completing monthly task work across approved categories. Rewards may include **GRAPE, USDC, and SOL**, depending on the category and monthly budget configuration.
{% endstep %}

{% step %}
### Merit (Period) Rewards

Earned through period-based recognition. In GREMS, the calculated merit-period emissions result in **GRAPE and USDC** rewards.
{% endstep %}

{% step %}
### Governance Participation Rewards (Standalone)

Earned through participation signals such as eligibility, voting, calls, and raffles. Governance participation outputs can produce **VINE Rewards** and roll up into a standalone payout artifact called **GOVERNMENT PARTICIPATION REWARDS**.
{% endstep %}
{% endstepper %}

## **3. Types of Reward Emissions and Prizes**

**VINE**

* Earned through governance participation outcomes (e.g., eligibility, voting, calls, raffles).
* More VINE can increase the chance of winning prizes (where applicable).
* VINE resets annually.

**GRAPE, USDC, SOL**

* **Performance (Monthly)** emissions may include GRAPE, USDC, and SOL, depending on category rules and budgets.
* **Merit (Period)** emissions in this system design include **GRAPE and USDC**.

**Other Rewards**

* Includes NFTs or other tokens (e.g., GRAPE, JUP, Bonk, etc.) awarded in raffles or as seasonal bonuses, when applicable.

## **4. Understanding the Sheets and Their Purpose**

The following is a list of GREMS spreadsheets and how data flows through them.

### **4.1 Inputs (Upstream Sources)**

**Budget pools per category**

* Approved budget allocations per category for a given cycle.
* Feeds the scoring sheets that calculate emissions.

**GRAPE MEMBERS (member roster/identifiers)**

* Canonical list of participating members and identifiers used across payout tables.
* Feeds reference sheets to ensure totals map to the correct member identity.

**Member Participation & Eligibility Spreadsheets**

* Separate inputs capture governance participation signals.
* These spreadsheets feed **GOV PARTICIPATION** (not VINE Rewards directly).

### **4.2 Governance Participation (Standalone Pipeline)**

**GOV PARTICIPATION**

* Processes governance participation inputs (eligibility, voting, calls, raffles).
* Outputs flow to **VINE Rewards** and to the governance payout artifact.

**VINE Rewards**

* Derived from GOV PARTICIPATION outcomes.
* Feeds the governance payout artifact/proposal.

**GOVERNMENT PARTICIPATION REWARDS**

* Standalone governance participation payout artifact.
* Consolidates outputs from **GOV PARTICIPATION** and **VINE Rewards**.
* Does **not** flow into the Emissions Results rollup reports in the current design.

### **4.3 Performance (Monthly) Emissions**

**SCORE: PERFORM & GUBA EMIT**

* Records monthly performance levels per task category for each member.
* Calculates **GRAPE / USDC / SOL** emissions based on approved category budgets and scoring logic.
* Outputs member totals to REF sheets.

**REF: PERFORM**

* Tabulates final monthly **GRAPE totals** per member from the SCORE sheet.
* Provides payout-ready GRAPE totals.

**REF: GUBA**

* Tabulates final monthly **USDC + SOL totals** per member from the SCORE sheet.
* Provides payout-ready USDC and SOL totals.

**REF: PROPOSAL ARCHITECT**

* Tracks monthly proposal counts, proposal authors, and instructions per proposal.
* Calculates **SOL refunds + rewards** for proposal architects.
* Output flows to **RECURRING PAYMENTS**.

**REF: DISCORD BOOST**

* Monitors Discord boosts donated by members to the GRAPE DAO Discord server.
* Converts boosts into **USDC rewards** per member.
* Output flows to **RECURRING PAYMENTS**.

### **4.4 Merit (Period) Emissions**

**SCORE: MERIT EMIT & GUBA MERIT**

* Records period performance levels for each member in each category.
* Calculates **GRAPE / USDC** emissions based on approved budgets and merit scoring logic.
* Outputs member totals to REF sheets.

**REF: MERIT**

* Tabulates final period **GRAPE totals** per member from the merit SCORE sheet.

**REF: GUBA MERIT**

* Tabulates final period **USDC totals** per member from the merit SCORE sheet.

### **4.5 Payments and Reporting**

**RECURRING PAYMENTS (monthly payment list)**

* Consolidates monthly payout instructions and totals from:
  * **REF: PROPOSAL ARCHITECT** (SOL refunds + rewards)
  * **REF: DISCORD BOOST** (USDC rewards)
  * Other DAO Infrastructure and Content Tool costs
* Output flows to proposals.

**EMISSIONS RESULTS - Performance (monthly rollup report)**

* Consolidates monthly payout instructions and totals from:
  * **REF: PERFORM** (GRAPE totals)
  * **REF: GUBA** (USDC + SOL totals)
* Output flows to proposals.
* Used for transparency and accounting of monthly distributions.

**EMISSIONS RESULTS - Merit (period rollup report)**

* Consolidates monthly payout instructions and totals from:
  * **REF: MERIT** (GRAPE totals)
  * **REF: GUBA MERIT** (USDC totals)
* Output flows to proposals.
* Used for transparency and accounting of period (merit) distributions.

## **5. Proposals and Artifacts That Distribute Rewards**

The following distribution common artifacts/proposals present and execute rewards upon community approval:

{% stepper %}
{% step %}
### RECURRING PAYMENTS (Monthly Distribution List)

* Funding Requests for Infrastructure, Content Tools, and Discord Boosts Rewards (USDC and SOL)
{% endstep %}

{% step %}
### EMISSION RESULTS PAYMENTS (Monthly Distribution List) - Performance

* GUBA Graperator Performance Emission (USDC & SOL)
* GRIP Graperator Performance Emission (GRAPE)
{% endstep %}

{% step %}
### EMISSION RESULTS PAYMENTS (Period Distributions List) - Merit

* GUBA Merit Rewards (USDC)
* GRAPE Merit Rewards (GRAPE)
{% endstep %}

{% step %}
### GOVERNMENT PARTICIPATION REWARDS (Standalone Governance Payout)

* Grape DAO Governance Participation (GRAPE)
* Grape DAO Governance Participation (VINE)
{% endstep %}

{% step %}
### Other Proposals

* Bounties & Special Projects
{% endstep %}
{% endstepper %}

## **6. Performance & Merit Task Categories**

The two lists below (Performance and Merit) list the task categories where Graperators can contribute. A Graperator may participate in none, some, or all categories, and each community member sets their participation limits.

### **6.1 Performance Categories (Monthly)**

Approved Proposal Architect\
Metrics: Work (Monitor)\
Moderate (Discord / Calls)\
Call Documentation Upkeep\
Moderate (Discourse / Admin)\
Product UAT DAO\
Quality Control Quorum Committee\
Creative Content & Videography\
Product Updates - Code Pushes\
Exploratory Committee (e.g., AMA research)\
Partner Communications\
DAO & Community Documentation\
DAO Metrics\
Event Calendar Management\
Syndicate Metrics\
Workshop Organizer\
Social Media (X)\
Discord Admin\
Research / Tokens / NFTs\
Treasury Controller\
Community Coordinator: Communication between GRAPE and non-GRAPE members.

### **6.2 Merit Categories (Period)**

Treasury Growth (USDC, SOL, OTHER)\
Tool Building\
Tool Usage\
Community Growth\
Resource Size / NFTs\
Brand Recognition\
Research\
Excellence\
Opportunity Creation\
DAO DOER\
Consistency

_Note:_ The Merit and Performance categories are adjusted periodically and can change. Check the latest set of spreadsheets for the latest updates.

## **6.3 Governance Participation Categories (Standalone)**

Governance Participation categories represent engagement signals used to determine eligibility and participation-based rewards (including VINE and governance participation rewards). These categories are tracked through the **Member Participation & Eligibility Spreadsheets**, processed in **GOV PARTICIPATION**, and may produce outputs in **VINE Rewards** and **GOVERNMENT PARTICIPATION REWARDS**.

**Governance Participation Categories**

{% stepper %}
{% step %}
### Attendance at Meetings

Participation in official GRAPE DAO meetings (as defined by the DAO schedule and attendance rules).
{% endstep %}

{% step %}
### Voting on Proposals

Participation in governance voting during active proposal windows, where applicable.
{% endstep %}

{% step %}
### Eligibility to Earn Rewards

Meeting baseline participation requirements (e.g., membership status, minimum stake thresholds, compliance with DAO guidelines) is needed to qualify for governance-related rewards.
{% endstep %}

{% step %}
### Participating in Raffles

DAO-defined rules govern participation in raffles and community engagement events. These are generally open to all meeting attendees and don’t require eligibility.
{% endstep %}
{% endstepper %}

_Note:_ The tracking and consolidation of Governance Participation rewards happen in a standalone pipeline controlled by the **GOVERNMENT PARTICIPATION REWARDS** sheets, separate from the monthly and periodic emissions rollup reports.

## **7. Conclusion: Empowering Participation**

It all begins with the Graperators. **GREMS** exists to recognize contributions, validate participation through transparent review, and help distribute rewards fairly across the GRAPE DAO.

The **GRIPv2 Emission & VINE Rewards spreadsheets** are designed to be transparent, structured, and continuously improved as the DAO evolves. They combine community oversight with calculation frameworks that track budgets, enforce caps, validate participants, and confirm allocations across monthly and period-based reward cycles.

Whether you are a first-time volunteer or a seasoned contributor, becoming a Graperator opens the door to meaningful impact, valuable rewards, and deeper integration into a growing Solana-based community. Join by attending weekly meetings, learning from the community, and volunteering—then help build the GRAPE DAO’s future, one task and one reward at a time. 🍇
