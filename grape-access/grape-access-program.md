# Grape Access Program

Program ID: GPASSzQQF1H8cdj5pUwFkeYEE4VdMQtCrYtUaMXvPz48

Network: Solana

Framework: Anchor

***

### Overview

Grape Access Protocol (GPASS) is a modular, on-chain access control layer for DAOs, communities, and protocols on Solana.

It enables programmable access gating based on:

* Reputation (OG Reputation Spaces)
* Verified identities (Grape Verification)
* Token holdings
* NFT collections
* Multi-DAO membership
* Time locks
* Custom external program validation

GPASS acts as a universal, composable access engine for the Grape ecosystem and beyond.

***

## 🎯 What Problem It Solves

Traditional gating mechanisms are:

* Off-chain
* Centralized
* Single-condition (token only, NFT only)
* Not composable
* Difficult to extend

GPASS solves this by providing:

* Fully on-chain validation
* Multi-condition logic (AND / OR)
* Cross-program verification
* Reusable access proofs
* Extensibility for future protocols

It transforms access control into programmable infrastructure.

***

## 🧱 Core Concepts

\
1️⃣ Access Space

An Access Space is a configurable rule set that defines who can access something.

It is stored in an Access account:

```rust
pub struct Access {
    pub version: u8,
    pub access_id: Pubkey,
    pub authority: Pubkey,
    pub criteria: AccessCriteria,
    pub access_type: AccessType,
    pub metadata_uri: String,
    pub is_active: bool,
    pub created_at: i64,
    pub total_checks: u64,
    pub successful_checks: u64,
    pub bump: u8,
}
```

Each Access Space:

* Has an authority
* Defines criteria
* Emits events
* Tracks usage metrics
* Can be paused or closed

***

### 2️⃣ Access Criteria (The Gating Engine)

GPASS supports multiple access types through the AccessCriteria enum.

#### Supported Criteria

#### 🔹 Minimum Reputation (OG Reputation)

```rust
MinReputation {
    vine_config: Pubkey,
    min_points: u64,
    season: u16,
}
```

Requires:

* Verified reputation PDA from Vine
* Minimum points threshold
* Season validation

***

#### 🔹 Verified Identity (Grape Verification)

```rust
VerifiedIdentity {
    grape_space: Pubkey,
    platforms: Vec<u8>,
}
```

Requires:

* Verified identity account
* Optional expiration check
* Platform match (Discord, Twitter, etc.)

***

#### 🔹 Verified Identity + Wallet Link

```rust
VerifiedWithWallet
```

Requires:

* Identity
* Wallet link PDA
* Cross-validation between identity and wallet

***

#### 🔹 Combined Reputation + Identity

```rust
Combined { ... }
```

Enables:

* Reputation AND identity checks
* Optional wallet link requirement
* Multi-layer Sybil resistance

***

#### 🔹 Time-Locked Reputation

```rust
TimeLockedReputation
```

Requires:

* Minimum reputation
* Held for X seconds
* Prevents flash reputation abuse

***

#### 🔹 Multi-DAO Gating

```rust
MultiDao {
    required_access_spaces: Vec<Pubkey>,
    require_all: bool,
}
```

Allows:

* Recursive access validation
* AND / OR logic
* Cross-community composability

Example:

> “Must pass DAO A AND DAO B access”

***

#### 🔹 Token Holding

```rust
TokenHolding {
    mint,
    min_amount,
    check_ata,
}
```

Supports:

* Any SPL token
* Optional strict ATA ownership
* Minimum balance requirement

***

#### 🔹 NFT Collection Gating

```rust
NftCollection {
    collection_mint,
    min_count,
}
```

Validates:

* Collection membership
* Verified metadata
* Minimum NFT count

***

#### 🔹 Custom Program Validation (Extensibility)

```rust
CustomProgram {
    program_id,
    instruction_data,
}
```

Enables:

* CPI-based validation
* External protocol integration
* Fully extensible logic

This makes GPASS future-proof.

***

## 🔄 Access Types

Defined by AccessType:

```rust
pub enum AccessType {
    SingleUse,
    Reusable,
    TimeLimited { duration_seconds },
    Subscription { interval_seconds },
}
```

Supports:

* One-time validation
* Reusable passes
* Time-bound passes
* Subscription-style access

This allows monetized or dynamic access models.

***

## 🧾 Access Check Records

When check\_access runs, it can create a reusable:

```rust
AccessCheckRecord
```

This:

* Stores check result
* Records timestamp
* Enables multi-access composition
* Prevents redundant computation
* Allows verification within 1 hour

<br>

Used heavily in MultiDao scenarios.

***

## 🔐 Security Architecture

<br>

### 1️⃣ Cross-Program Validation

GPASS validates:

* Reputation PDAs (Vine)
* Identity PDAs (Grape Verification)
* Link PDAs
* Token accounts
* NFT metadata
* Custom validation accounts

It verifies:

* Owner program IDs
* PDA derivations
* Season consistency
* Space consistency
* Expiration
* Instruction hash integrity

This prevents spoofed accounts.

***

### 2️⃣ Anti-Sybil Mechanisms

GPASS supports:

* Reputation thresholds
* Time-locks
* Identity verification
* Wallet linking
* Cross-DAO requirements

It allows communities to design advanced Sybil resistance systems.

***

### 3️⃣ Emergency Admin Close

```rust
pub fn admin_close_any
```

Emergency account recovery mechanism:

* Only callable by ADMIN
* Can close program-owned accounts
* Used for upgrade recovery scenarios

***

## 📊 Built-In Metrics

Each Access Space tracks:

* total\_checks
* successful\_checks

This enables:

* Analytics
* Monitoring
* Community transparency
* Access demand measurement

***

## 🧠 Integration With Grape Ecosystem

GPASS integrates directly with:

* Vine Reputation Program
* Grape Verification Registry

This creates a layered identity + reputation + wallet binding system.

Together they form:

> Identity → Reputation → Access

A fully on-chain trust stack.

***

## 🏗 Example Use Cases

DAO Treasury Voting Access

* Must hold 1,000 reputation points
* Must have verified Discord
* Must have wallet linked

***

Exclusive NFT Event

* Hold 2 NFTs from collection
* Or pass access of Partner DAO

***

Paid Subscription Content

* Hold 100 tokens
* AccessType: Subscription (30 days)

***

Governance Contribution Reward Access

* Reputation ≥ 500
* Held for 30 days
* Verified identity

***

## 🚀 Benefits

\
1️⃣ Fully On-Chain

No backend servers required.

***

2️⃣ Composable

Access spaces can validate other access spaces.

***

3️⃣ Extensible

Custom program validation makes it future-proof.

***

4️⃣ Secure

Strict PDA validation and program ownership checks.

***

5️⃣ Modular

Criteria can be updated by authority.

***

6️⃣ Analytics-Friendly

Tracks usage and success rates.

***

### 7️⃣ Anti-Abuse Ready

Supports:

* Time locks
* Identity expiration
* Wallet linking
* Multi-DAO AND/OR logic

***

## 🌐 Why This Matters

GPASS turns access control into programmable infrastructure.

Instead of:

> “Do they hold a token?”<br>

We can now ask:

> “Are they verified?

> Do they contribute?

> Do they hold tokens?

> Are they trusted across multiple DAOs?

> Have they held reputation long enough?”

This is reputation-aware, identity-aware access control.

***

## 🔮 Future Possibilities

* Real-world credential gating
* AI reputation scoring integration
* Cross-chain identity bridges
* On-chain subscription monetization
* DAO federation models
* Dynamic reputation decay gating

***

🏁 Conclusion\
Grape Access Protocol (GPASS) is a universal access control engine for Solana DAOs and communities.
---------------------------------------------------------------------------------------------------

It provides:

* Composable gating
* Reputation integration
* Identity validation
* Token/NFT support
* Extensibility
* Security-first architecture

It forms the foundation for:

> Trust-based, programmable community infrastructure.
