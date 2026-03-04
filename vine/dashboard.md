# Dashboard UI

OG Reputation is a season-based, on-chain reputation system that helps DAOs recognize meaningful participation without turning reputation into financial value or increasing governance power. Reputation is tied to governance identity (wallet), is transparent, and is designed to be resettable by season while keeping historical context.

Think of it as a Web3-native participation layer: track, reward, and showcase contribution over time—without minting a “power token.”

***

### Core concepts

#### Seasons

Reputation is organized into seasons. Each season is a fresh cycle that:

* starts clean for new participation,
* preserves history from prior seasons,
* makes it easy to run recurring programs (quarters, epochs, campaigns, working groups, etc.).

#### Spaces (DAO instances)

A Reputation Space is an on-chain configuration tied to a DAO/governance identity. A space defines:

* the DAO/program it belongs to,
* the current season,
* optional metadata (name, symbol, branding/theme).

Spaces can be browsed publicly in the Reputation Directory.

***

### What the UI includes

#### Reputation Directory

Browse existing spaces in a clean “gallery” view. Each card can include:

* DAO name + symbol
* season number
* DAO id (shortened)
* branding (logo + optional background image/theme)

From the directory you can:

* open a space dashboard,
* create a new space (wallet connect required).

#### Space Dashboard

Each space dashboard can include several modules depending on configuration:

Leaderboard

* Shows token holders (or participants) ranked by balance/weight.
* Displays summary stats (supply, decimals, total held, holder counts, concentration metrics, etc.).
* Includes wallet drill-down (drawer) for a holder profile.

Holder Profile

* View a wallet’s rank, balances, % of supply, tier, and estimated draw chance (when applicable).
* Quick links (e.g., Solscan / Governance.so).
* Embedded Vine Reputation view for that wallet (season-aware).

Exports

* Copy/download CSV for importing into tools or for reputation distribution workflows.
* Copy/share formatted outputs (e.g., Markdown for announcements).

Randomizer (optional UI section)

* A raffle-style draw tool (weighted by balance) for giveaways or participation rewards.
* Can run in a “stream mode” for screen share/Discord.
* Supports multiple winners, excludes treasury/system wallets, and avoids repeat winners.

***

### Why DAOs use OG Reputation

* Recognize contribution without financializing reputation
* Seasonal structure for recurring incentives and clean cycles
* Public accountability (on-chain identity, transparent rules)
* Composable workflows (exportable data, integrations, shareable outputs)
* Brandable experience per DAO space (logos, themes, backgrounds)



<figure><img src="../.gitbook/assets/Screenshot 2026-01-21 at 11.18.14 AM.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Screenshot 2026-01-21 at 11.18.36 AM.png" alt=""><figcaption></figcaption></figure>



{% embed url="https://reputation.governance.so" %}

{% embed url="https://github.com/Grape-Labs/grape-vine-dashboard" %}
