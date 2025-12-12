# Vine Reputation Program

### Program Scope

The VINE Reputation Program is an on-chain reputation framework designed to manage season-based participation scores for a DAO.

It defines:

* How reputation spaces are created
* How reputation is issued and tracked
* How seasons are managed
* How administrative control is enforced

This program operates independently of governance voting logic and can be integrated into multiple DAO workflows.

***

### Core On-Chain Components

Reputation Space (Config)

A Reputation Space is a DAO-scoped configuration account that defines:

* DAO identifier (realm / governance address)
* Authority wallet
* Reputation mint reference
* Current active season
* Program-level settings

Each DAO has exactly one reputation space.

***

#### Reputation Accounts

Reputation is tracked per:

* DAO
* User wallet
* Season

Each (DAO, User, Season) tuple maps to a unique on-chain account holding the reputation balance for that season.

Key properties:

* Immutable past seasons
* Writable only for the active season
* Authority-gated updates

***

#### Project Metadata

Each reputation space may optionally store:

* A metadata URI
* Off-chain JSON describing name, icon, links, or descriptive information

This allows frontends and dashboards to display DAO-specific context without hardcoding values.

***

### Season Model

The program uses a monotonic season counter:

* Seasons are identified by a u16 value
* Only one season is active at any time
* Advancing the season does not modify historical data

When a new season is set:

* New reputation accrues to new accounts
* Old reputation remains unchanged and queryable

This model prevents reputation inflation while preserving historical integrity.

***

### Authority Model

Each reputation space defines an authority wallet that can perform administrative actions.

Authority-gated actions include:

* Setting the active season
* Updating the reputation mint
* Issuing or resetting reputation
* Transferring reputation between wallets
* Updating metadata
* Closing accounts

Authority can be transferred at any time via an on-chain instruction.

***

### Reputation Issuance

Reputation may be issued in several ways:

* Single-user allocations
* Batch allocations
* Imported historical data (e.g. CSV backfills)
* Automated workflows

All reputation issuance:

* Is explicitly authorized
* Is recorded on-chain
* Is scoped to a specific season

***

### Reputation Mutations

The program supports controlled reputation mutations:

| Action              | Description                                             |
| ------------------- | ------------------------------------------------------- |
| Add reputation      | Increment a user’s balance for the active season        |
| Reset reputation    | Reset a user’s balance to zero                          |
| Transfer reputation | Move reputation between wallets (e.g. wallet migration) |
| Close reputation    | Close unused or incorrect accounts                      |

These operations do not affect other seasons.

***

### Account Closure

For operational hygiene, the program supports closing:

* Reputation accounts
* Project metadata accounts
* Entire reputation spaces

Closed accounts:

* Return rent to a designated recipient
* Permanently remove the account from use
* Do not alter historical data already finalized on-chain

***

### Data Integrity Guarantees

The program enforces:

* PDA-derived account addresses
* Deterministic account relationships
* Authority validation on all writes
* Season-scoped isolation of balances

This ensures data consistency and auditability.

***

### Network Deployment

The VINE Reputation Program is deployed on:

* Devnet for testing and iteration
* Mainnet for production DAO usage

Program behavior is identical across networks.

***

### Integration Surface

The program is designed to integrate with:

* DAO dashboards
* Governance analytics
* Participation tracking tools
* Automation pipelines

Frontends may:

* Query historical seasons
* Aggregate reputation data
* Visualize leaderboards
* Enforce governance logic externally

***

### Summary

The VINE Reputation Program provides a deterministic, season-based, authority-governed reputation system for DAOs.

It separates participation tracking from governance power while enabling transparent, auditable, and flexible reputation management on-chain.

{% embed url="https://vine.governance.so" %}

