# About

<figure><img src="../.gitbook/assets/apple-touch-icon.png" alt=""><figcaption></figcaption></figure>

### About OG Reputation Spaces

OG Reputation is Grape’s reputation infrastructure for Solana communities.\
It gives DAOs a clear, on-chain way to track participation over time, apply seasonal reputation logic, and turn that data into usable community signals for governance, recognition, and incentives.

### What We’ve Built

We’ve built the Vine Dashboard as a complete operating layer for DAO reputation:

* **Reputation Spaces:** Create and manage DAO-specific reputation spaces tied to on-chain config.
* **Admin Controls:** Update seasons, decay settings, authority, and metadata; add, reset, transfer, and bulk-import reputation.
* **Metadata + Branding:** Upload and manage project metadata (including theme data and assets) with storage integration.
* **Leaderboards:** Run both token-based and reputation-based leaderboards with export tools and weighted randomizer flows.
* **Shareable Reputation Cards:** Generate wallet/DAO reputation cards and dynamic social preview images.
* **DAO Discovery + Routing:** Browse spaces in a directory and access dedicated per-DAO dashboard routes.
* **PWA Experience:** Support installable app behavior with dynamic manifest and DAO-branded icons.
* **Wallet + RPC Flexibility:** Integrate Solana wallets and configurable RPC endpoints for reliable access across environments.

OG Reputation turns raw participation data into a branded, transparent, and actionable reputation product for DAOs.

***

### What OG Reputation Includes

#### 🟢 1. Mainnet Reputation Protocol

OG runs live on Solana Mainnet, where:

* Each DAO has its own Reputation Space
* Reputation is tracked per wallet, per season
* All allocations are recorded on-chain
* Seasons preserve historical state immutably
* Admin actions are permissioned but publicly auditable

The system is optimized for transparency, scalability, and multi-DAO deployment.

***

#### 📦 2. Developer SDK (NPM Package)

OG provides an official NPM SDK that allows developers to:

* Create and manage Reputation Spaces
* Allocate or adjust reputation
* Start and close seasons
* Query user reputation on-chain
* Integrate OG into dApps, dashboards, or analytics tools

This enables seamless integration into:

* Governance dashboards
* Community platforms
* Participation tracking tools
* Custom reward systems

***

#### 🤖 3. Discord Bot Integration

OG includes a production-ready Discord Bot that connects off-chain participation with on-chain reputation.

The bot can:

* Award reputation based on Discord activity
* Automate seasonal allocations
* Gate roles based on reputation
* Prevent abuse through configurable permissions
* Operate in fail-closed mode for governance safety

This allows communities to bridge Web2 participation with Web3 reputation — securely and transparently.

***

### How It Works

#### Reputation Spaces

* Each DAO or working group has a dedicated on-chain Reputation Space
* Spaces are derived from governance identifiers
* Spaces support metadata (name, icon, links)

#### Seasons

* Reputation is tracked per season
* New seasons create a clean participation ledger
* Previous seasons remain immutable and queryable
* DAOs can evolve allocation criteria per season

#### Allocation

Reputation can be:

* Manually allocated by authorized admins
* Bulk-imported (CSV support)
* Automated via Discord bot
* Integrated programmatically via SDK

***

### Why OG Reputation Exists

DAOs need a system that:

* Measures contribution beyond token balance
* Avoids inflationary governance models
* Preserves historical participation
* Supports automation
* Scales across multiple communities

OG solves this with programmable, season-based, non-financial reputation.

***

### Transparency & Integrity

* All reputation transactions are on-chain
* Metadata is cryptographically referenced
* Admin actions are publicly auditable
* No hidden adjustments
* Designed for long-term governance integrity

***

### Key Principles

* 🛡 Non-financial
* 🔍 Fully auditable
* 🔄 Season-based
* 🤖 Automation-ready
* 🧩 Multi-DAO scalable
* ⚡ Mainnet deployed

***

### Points of Attention

* OG Reputation does not represent financial value
* It does not replace governance tokens
* It complements governance by measuring contribution
* Admin authority is transparent and permissioned
* Designed to evolve without breaking historical data
