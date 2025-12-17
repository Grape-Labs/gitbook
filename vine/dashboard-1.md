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

### Seasonal Reputation Decay

Vine Reputation uses a seasonal decay model to ensure that reputation reflects _recent participation_ while still honoring historical contributions.

Instead of permanently accumulating reputation forever, older seasons gradually lose influence over time.

***

### Core Concept

Each DAO operates in seasons (integer-based: 1, 2, 3, …).

* Reputation is earned per season
* Older seasons are discounted using a decay formula
* The current season always has full weight (1.0)

The effective reputation is the sum of all past seasons after applying decay.

***

### Decay Formula

We store a single decay rate on-chain for the DAO:

```
decay = 0.30   (30% decay per season)
```

The weight applied to a season is calculated as:

```
weight = (1 - decay) ^ seasons_ago
```

Where:

* seasons\_ago = current\_season - season
* decay is a value between 0.0 and 1.0

***

### Example

Assume:

* Current season = 3
* Decay = 30% (0.30)
* Reputation earned:

| Season | Raw Points |
| ------ | ---------- |
| 3      | 95         |
| 2      | 70         |
| 1      | 59         |

#### Calculated weights

| Season | Seasons Ago | Weight | Effective      |
| ------ | ----------- | ------ | -------------- |
| 3      | 0           | 1.00   | 95 × 1.00 = 95 |
| 2      | 1           | 0.70   | 70 × 0.70 = 49 |
| 1      | 2           | 0.49   | 59 × 0.49 ≈ 29 |

#### Totals

```
Total (raw):     224
Total (decayed): 173
```

### Why This Model?

This approach provides:

* Recency bias — recent activity matters more
* Fairness — early contributors are still rewarded
* Predictability — simple, transparent math
* On-chain efficiency — no per-season storage overhead
* Only raw reputation is stored on-chain.
* Decay is applied deterministically off-chain (UI, indexer, analytics).

***

### On-Chain vs Off-Chain Responsibilities

\
On-Chain

* Stores:
  * Raw reputation per (DAO, user, season)
  * Current season
  * Decay rate
* Guarantees:
  * Authority control
  * Season isolation
  * Data integrity

\
Off-Chain (UI / Indexer)

* Calculates:
  * Seasonal weights
  * Effective reputation
  * Aggregated totals
* Displays:
  * Per-season breakdown
  * Decayed totals
  * Historical contribution

\
This keeps the program cheap, flexible, and future-proof.

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

