# Grape Access Console UI

Overview

Grape Access Console is a community access management UI for Solana. It helps operators create and manage on-chain access gates, while giving members a self-serve page to check whether they qualify for access.

The UI is designed for communities that want transparent, programmable access rules without relying on manual moderator checks.

### What This UI Does

#### 1. Create Gate (Policy Builder)

Community admins can create new gates with a guided builder that supports templates and advanced options.

Supported policy types include:

* Reputation + verified identity
* Reputation-only
* Verified identity (with optional wallet link)
* Token holder
* NFT collection holder
* Multi-DAO and custom program criteria

Gate behavior can also be configured as:

* Single use
* Reusable
* Time-limited
* Subscription-based

#### 2. Check Access (Operator/Moderator Tools)

Moderators can run live checks against any wallet, inspect pass/fail results, and use advanced account overrides for troubleshooting.

This helps support teams quickly diagnose why a member passed or failed a gate check.

#### 3. Admin Console (Lifecycle Management)

Admins can manage existing gates from one place:

* Load gates by authority wallet
* Fetch gate details
* Update metadata URI
* Activate/deactivate gate behavior
* Transfer gate authority
* Close check records or close gates

#### 4. Member Portal (`/access`)

Members get a dedicated self-serve page where they can:

* Connect wallet
* Enter or open with a prefilled gate ID
* Auto-derive required accounts
* Run **Check My Access**
* See proof/context links and clear pass/fail messaging

Communities can share deep links like `/access?gateId=<GATE_ID>` so users land directly in the right flow.

### Why This Benefits a Community

#### Reduced moderator workload

The member self-serve flow and auto-derive tools reduce repetitive “am I eligible?” tickets in Discord/Telegram.

#### Transparent and auditable access

Checks run against on-chain gate logic, which improves trust and consistency versus manual allowlists.

#### Faster onboarding for new members

Deep links and guided UI steps make entry simple for non-technical users.

#### Flexible access policy design

Communities can choose criteria that match their model:

* Social verification communities
* Reputation-weighted DAOs
* Token or NFT membership groups
* Hybrid models that combine multiple requirements

#### Safer operations for admins

The Admin Console gives structured controls for updates and lifecycle actions, reducing operational mistakes.

### Example Community Use Cases

* A DAO grants proposal channel access only to members with minimum reputation and verified identity.
* An NFT club auto-qualifies members holding assets from a specific collection.
* A token community creates reusable access checks for holder-only events.
* Moderators validate edge cases in real time without backend engineering support.

### Typical Community Workflow

1. Admin connects authority wallet and selects the correct network.
2. Admin creates a gate from a template and configures criteria.
3. Team reviews payload and initializes gate on-chain.
4. Community shares member link (`/access?gateId=...`).
5. Members self-check eligibility and complete verification if needed.
6. Moderators use Check Access for support and exception handling.
7. Admins manage updates/lifecycle in the Admin Console.

### Summary

Grape Access Console gives communities a practical way to run access control as a transparent on-chain process, while keeping the user experience simple for both operators and members.
