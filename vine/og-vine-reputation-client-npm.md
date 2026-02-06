# OG/Vine Reputation Client (NPM)

We’ve released an official Vine Reputation JavaScript client on NPM to make integrating reputation into apps, dashboards, and DAOs simple, composable, and framework-agnostic.

This client removes the need to:

* parse Anchor IDLs manually
* write custom PDA / discriminator logic
* maintain duplicated decoding code across projects

\
Instead, it provides a clean, typed API for interacting with the Vine Reputation program.

***

#### 📦 Package

NPM:

https://www.npmjs.com/package/@grapenpm/vine-reputation-client

***

#### ✨ What this gives you

* ✅ Typed helpers for PDAs
* ✅ Safe decoders for config, reputation, and metadata accounts
* ✅ Instruction builders for creating and managing reputation
* ✅ Works in React, Next.js, Node, and serverless
* ✅ No Anchor dependency required

<br>

This is the same client used internally by the Vine Dashboard.

#### Installation

```bash
npm install @grapenpm/vine-reputation-client
```

or

```bash
yarn add @grapenpm/vine-reputation-client
```

Program Info

```bash
import { VINE_REP_PROGRAM_ID } from "@grapenpm/vine-reputation-client";
```

* Network: Mainnet & Devnet Support
* Program ID: V1NE6WCWJPRiVFq5DtaN8p87M9DmmUd2zQuVbvLgQwX

***

#### 🔍 Common Use Cases

* Display per-season reputation for a wallet
* Build leaderboards and governance weighting
* Fetch and render DAO reputation configs
* Create and manage reputation spaces
* Attach metadata (titles, images, descriptions) to spaces

***

#### 📖 Full API & Examples

For the complete API surface, examples, and up-to-date usage instructions, visit the package page on NPM:

👉 https://www.npmjs.com/package/@grapenpm/vine-reputation-client

***

#### 🧩 Designed for Composition

The Vine Reputation client is intentionally low-level and composable.

It can be used on its own or combined with:

* Governance UIs
* DAO dashboards
* Analytics pipelines
* Custom voting or weighting systems

No framework lock-in. No magic.
