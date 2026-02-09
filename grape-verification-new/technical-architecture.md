# Technical Architecture

## System Overview

Grape Verification is built on Solana using the Anchor framework. The system consists of on-chain programs, client libraries, and integration tools.

```
┌─────────────────────────────────────────────────────────┐
│                    User Interface                        │
│              verification.governance.so                  │
└────────────────┬────────────────────────────────────────┘
                 │
                 ↓
┌─────────────────────────────────────────────────────────┐
│                   Attestor API                           │
│        Verifies platform identity + submits tx           │
└────────────────┬────────────────────────────────────────┘
                 │
                 ↓
┌─────────────────────────────────────────────────────────┐
│            Solana Blockchain (Mainnet)                   │
│         Program: VrFyyRxPoyWxpABp...                    │
│  ┌──────────┐  ┌───────────┐  ┌──────────┐            │
│  │  Space   │─→│ Identity  │─→│   Link   │            │
│  │ Account  │  │  Account  │  │ Account  │            │
│  └──────────┘  └───────────┘  └──────────┘            │
└─────────────────────────────────────────────────────────┘
```

### On-Chain Program

#### Program Information

* **Program ID**: `VrFyyRxPoyWxpABpBXU4YUCCF9p8giDSJUv2oXfDr5q`
* **Network**: Solana Mainnet Beta
* **Framework**: Anchor v0.29+
* **Language**: Rust
* **Upgradeable**: Yes (admin controlled)

#### Account Structure

**Space Account**

The root account for each DAO/community.

**Seeds**: `["space", dao_id]`

```rust
pub struct GrapeVerificationSpace {
    pub version: u8,           // Protocol version
    pub dao_id: Pubkey,        // DAO identifier
    pub authority: Pubkey,     // Admin who can update settings
    pub attestor: Pubkey,      // Authorized to create verifications
    pub is_frozen: bool,       // Emergency freeze flag
    pub bump: u8,              // PDA bump seed
    pub salt: [u8; 32],        // Random salt for hashing
    pub _padding: [u8; 5],     // Reserved for future use
}
```

**Size**: 144 bytes (8 discriminator + 136 data)

**Rent**: \~0.0015 SOL (one-time)

**Identity Account**

One per verified user per platform.

**Seeds**: `["identity", space_pubkey, platform_seed, id_hash]`

```rust
pub struct GrapeVerificationIdentity {
    pub version: u8,           // Protocol version
    pub space: Pubkey,         // Reference to Space
    pub platform: u8,          // 0=Discord, 1=Telegram, 2=Twitter, 3=Email
    pub id_hash: [u8; 32],     // SHA256(salt || platform_tag || user_id)
    pub verified: bool,        // Verification status
    pub verified_at: i64,      // Unix timestamp
    pub expires_at: i64,       // Expiration (0 = never)
    pub attested_by: Pubkey,   // Attestor who verified
    pub bump: u8,              // PDA bump seed
    pub _padding: [u8; 4],     // Reserved
}
```

**Size**: 128 bytes (8 discriminator + 120 data)

**Rent**: \~0.0012 SOL (one-time)

**Link Account**

One per wallet linked to an identity.

**Seeds**: `["link", identity_pubkey, wallet_hash]`

```rust
pub struct GrapeVerificationLink {
    pub version: u8,           // Protocol version
    pub identity: Pubkey,      // Reference to Identity
    pub wallet_hash: [u8; 32], // SHA256(salt || "wallet" || wallet_pubkey)
    pub linked_at: i64,        // Unix timestamp
    pub bump: u8,              // PDA bump seed
    pub _padding: [u8; 6],     // Reserved
}
```

**Size**: 88 bytes (8 discriminator + 80 data)

**Rent**: \~0.0008 SOL (one-time, reclaimable on unlink)

### Cryptographic Design

#### Hash Function: SHA-256

All hashing uses SHA-256 for:

* Collision resistance
* One-way property (can't reverse)
* Industry standard security

#### Identity Hash

```
id_hash = SHA256(salt || platform_tag || platform_user_id)

where:
  salt = 32 random bytes from Space account
  platform_tag = "discord" | "telegram" | "twitter" | "email"
  platform_user_id = "123456789" (string representation)
```

**Example**:

```typescript
const salt = new Uint8Array(32); // from Space
const tag = new TextEncoder().encode('discord');
const userId = new TextEncoder().encode('123456789');

const input = concat(salt, tag, userId);
const id_hash = SHA256(input);
```

#### Wallet Hash

```
wallet_hash = SHA256(salt || "wallet" || wallet_pubkey_bytes)

where:
  salt = 32 random bytes from Space account
  "wallet" = constant tag (7 bytes UTF-8)
  wallet_pubkey_bytes = 32 bytes (Solana pubkey)
```

**Example**:

```typescript
const salt = new Uint8Array(32); // from Space
const tag = new TextEncoder().encode('wallet');
const pubkey = walletPublicKey.toBytes(); // 32 bytes

const input = concat(salt, tag, pubkey);
const wallet_hash = SHA256(input);
```

#### Salt Generation

Each Space has a unique 32-byte random salt:

```typescript
import crypto from 'crypto';

const salt = crypto.randomBytes(32);
```

**Purpose**:

* Prevents rainbow table attacks
* Makes hashes unique per DAO
* Can't correlate identities across different DAOs

### Instructions

#### 1. initialize\_space

**Authority**: DAO admin

**Purpose**: Create a new Space account for a DAO

```rust
pub fn initialize_space(
    ctx: Context<InitializeSpace>,
    dao_id: Pubkey,
    salt: [u8; 32],
) -> Result<()>
```

**Accounts**:

* `space_acct` \[writable, init] - PDA to create
* `authority` \[signer] - Becomes space authority
* `payer` \[signer, writable] - Pays rent
* `system_program` - System program

**Cost**: \~0.0015 SOL (rent) + transaction fee

#### 2. set\_space\_attestor

**Authority**: Space authority

**Purpose**: Update authorized attestor

```rust
pub fn set_space_attestor(
    ctx: Context<SetSpaceAttestor>,
    _dao_id: Pubkey,
    new_attestor: Pubkey,
) -> Result<()>
```

**Accounts**:

* `space_acct` \[writable] - Space to update
* `authority` \[signer] - Must match space.authority

**Cost**: Transaction fee only

#### 3. set\_space\_frozen

**Authority**: Space authority

**Purpose**: Emergency freeze/unfreeze

```rust
pub fn set_space_frozen(
    ctx: Context<SetSpaceFrozen>,
    _dao_id: Pubkey,
    frozen: bool,
) -> Result<()>
```

**When frozen**: No new verifications or links can be created.

#### 4. attest\_identity

**Authority**: Attestor

**Purpose**: Create or update identity verification

```rust
pub fn attest_identity(
    ctx: Context<AttestIdentity>,
    _dao_id: Pubkey,
    platform: VerificationPlatform,
    platform_seed: u8,
    id_hash: [u8; 32],
    expires_at: i64,
) -> Result<()>
```

**Accounts**:

* `space_acct` - Space reference
* `attestor` \[signer] - Must match space.attestor
* `identity` \[writable, init\_if\_needed] - Identity PDA
* `payer` \[signer, writable] - Pays rent if creating
* `system_program`

**Validation**:

* Attestor must match space.attestor
* platform\_seed must match platform enum
* Space must not be frozen

**Cost**: \~0.0012 SOL (if creating new) + transaction fee

#### 5. revoke\_identity

**Authority**: Attestor

**Purpose**: Revoke verification (doesn't delete links)

```rust
pub fn revoke_identity(
    ctx: Context<RevokeIdentity>,
    _dao_id: Pubkey,
    platform: VerificationPlatform,
    platform_seed: u8,
    id_hash: [u8; 32],
) -> Result<()>
```

Sets `verified = false`, `verified_at = 0`, `expires_at = 0`

#### 6. link\_wallet

**Authority**: Attestor

**Purpose**: Link a wallet to an identity

```rust
pub fn link_wallet(
    ctx: Context<LinkWallet>,
    _dao_id: Pubkey,
    platform_seed: u8,
    id_hash: [u8; 32],
    wallet_hash: [u8; 32],
) -> Result<()>
```

**Accounts**:

* `space_acct` - Space reference
* `attestor` \[signer] - Must match space.attestor
* `identity` - Must be verified and not expired
* `wallet` - Wallet to link (UncheckedAccount)
* `link` \[writable, init\_if\_needed] - Link PDA
* `payer` \[signer, writable] - Pays rent
* `system_program`

**Validation**:

* Attestor authorized
* Identity exists and verified
* Not expired (if expiration set)
* wallet\_hash matches SHA256(salt || "wallet" || wallet.key())
* Space not frozen

**Cost**: \~0.0008 SOL (if creating new) + transaction fee

#### 7. link\_wallet\_self

**Authority**: User's wallet

**Purpose**: User self-links wallet (identity must already be verified)

```rust
pub fn link_wallet_self(
    ctx: Context<LinkWalletSelf>,
    _dao_id: Pubkey,
    platform_seed: u8,
    id_hash: [u8; 32],
    wallet_hash: [u8; 32],
) -> Result<()>
```

**Difference from link\_wallet**:

* `wallet` must be a **Signer** (user signs transaction)
* No attestor signature required
* Identity must already be verified by attestor

**Use case**: User wants to link additional wallets after initial verification

#### 8. unlink\_wallet

**Authority**: Attestor

**Purpose**: Unlink a wallet and close the Link account

```rust
pub fn unlink_wallet(
    ctx: Context<UnlinkWallet>,
    _dao_id: Pubkey,
    platform_seed: u8,
    id_hash: [u8; 32],
    wallet_hash: [u8; 32],
) -> Result<()>
```

**Accounts**:

* `link` \[writable] - Closed after unlinking
* `recipient` \[writable] - Receives rent refund

**Effect**: Link account deleted, rent returned to recipient

**Rent Recovered**: \~0.0008 SOL

#### 9. admin\_close\_any

**Authority**: Admin (hardcoded pubkey)

**Purpose**: Emergency account closure

```rust
pub fn admin_close_any(
    ctx: Context<AdminCloseAny>
) -> Result<()>
```

**Admin pubkey**: `GScbAQoP73BsUZDXSpe8yLCteUx7MJn1qzWATZapTbWt`

Used only for emergency recovery or cleanup.

### Events

All state changes emit events for indexing:

```rust
#[event]
pub struct SpaceInitialized {
    pub space: Pubkey,
    pub dao_id: Pubkey,
    pub authority: Pubkey,
    pub attestor: Pubkey,
}

#[event]
pub struct IdentityUpdated {
    pub space: Pubkey,
    pub identity: Pubkey,
    pub platform: u8,
    pub verified: bool,
    pub verified_at: i64,
    pub expires_at: i64,
    pub id_hash: [u8; 32],
    pub attested_by: Pubkey,
}

#[event]
pub struct WalletLinked {
    pub space: Pubkey,
    pub identity: Pubkey,
    pub link: Pubkey,
    pub wallet_hash: [u8; 32],
    pub linked_at: i64,
}

#[event]
pub struct WalletUnlinked {
    pub space: Pubkey,
    pub identity: Pubkey,
    pub link: Pubkey,
    pub wallet_hash: [u8; 32],
    pub unlinked_at: i64,
}
```

### Security Model

#### Trust Assumptions

1. **Attestor Honesty**: The attestor correctly verifies platform identities
2. **Platform OAuth**: Discord/Telegram OAuth is secure
3. **Wallet Signatures**: Users control their private keys
4. **RPC Nodes**: Report accurate blockchain state

#### Attack Vectors & Mitigations

**Fake Verifications**

**Attack**: Malicious attestor creates fake verifications

**Mitigation**:

* Only one authorized attestor per Space
* DAO controls who the attestor is
* Can revoke attestor if compromised

**Rainbow Table**

**Attack**: Pre-compute hashes to identify users

**Mitigation**:

* Unique random salt per Space
* Makes pre-computation infeasible
* Can't correlate across different DAOs

**Hash Collision**

**Attack**: Find two inputs with same hash

**Mitigation**:

* SHA-256 collision resistance (2^128 operations)
* Computationally infeasible

**Front-running**

**Attack**: Observe pending transaction and submit own first

**Mitigation**:

* Idempotent operations (init\_if\_needed)
* No financial incentive to front-run
* PDA derivation ensures uniqueness

**Social Engineering**

**Attack**: Trick user into linking wrong wallet

**Mitigation**:

* User signs explicit consent message
* Consent message shows wallet address
* UI displays clear warnings

#### Privacy Analysis

**What's Private** (not on-chain):

* Actual Discord/Telegram user IDs
* Actual wallet addresses (in account data)
* Platform credentials
* Email addresses

**What's Public** (on-chain):

* Hashed identities
* Hashed wallets
* Verification timestamps
* Link existence

**Metadata Leakage**:

* Transaction signatures reveal wallet addresses
* Timing analysis could correlate verifications
* Number of links per identity is visible

**Recommendation**: Users who require full anonymity should use separate wallets for verification vs. transactions.

### Performance Characteristics

#### Transaction Costs

| Operation             | Rent                | Transaction Fee | Total            |
| --------------------- | ------------------- | --------------- | ---------------- |
| Initialize Space      | \~0.0015 SOL        | \~0.000005 SOL  | \~0.0015 SOL     |
| Attest Identity (new) | \~0.0012 SOL        | \~0.000005 SOL  | \~0.0012 SOL     |
| Link Wallet (new)     | \~0.0008 SOL        | \~0.000005 SOL  | \~0.0008 SOL     |
| Unlink Wallet         | Refund \~0.0008 SOL | \~0.000005 SOL  | Net: -0.0008 SOL |

**Total for full verification**: \~0.002 SOL (≈ $0.20 at $100/SOL)

#### RPC Calls

Typical verification check:

1. getAccountInfo(space) - Get salt
2. getAccountInfo(identity) - Check verification
3. getProgramAccounts(links) - Get linked wallets

**Total**: 3 RPC calls per check

#### Scalability

**Per Space**:

* Unlimited identities
* Unlimited links per identity
* No account limits

**Network**:

* Solana TPS: \~2,000-3,000 currently
* Verification tx: \~0.1% of block space
* Can handle millions of verifications per day

### Upgrade Path

#### Current Version: 2

**Version in all accounts**: `version: u8`

Allows:

* Protocol upgrades without data migration
* Backwards compatibility
* Feature flags

#### Planned Upgrades

**V3 (Future)**:

* Multi-attestor support
* Delegated attestation
* Batch operations
* Compressed account state

### Integration Points

#### Client Libraries

**JavaScript/TypeScript**:

```
npm install @grapenpm/grape-verification-registry
```

**Python** (planned):

```
pip install grape-verification
```

**Rust** (planned):

```
cargo add grape-verification-registry
```

#### RPC Endpoints

**Mainnet**:

* `https://api.mainnet-beta.solana.com` (rate limited)
* `https://mainnet.helius-rpc.com` (recommended)

**Devnet**:

* `https://api.devnet.solana.com`

#### Indexers

Currently being indexed by:

* Helius
* TheGraph (planned)
* Custom indexer (verification.governance.so)

### Monitoring

#### Key Metrics

```typescript
// Spaces created
const spaces = await connection.getProgramAccounts(PROGRAM_ID, {
  filters: [{ dataSize: 144 }]
});

// Total verifications
const identities = await connection.getProgramAccounts(PROGRAM_ID, {
  filters: [{ dataSize: 128 }]
});

// Total links
const links = await connection.getProgramAccounts(PROGRAM_ID, {
  filters: [{ dataSize: 88 }]
});
```

#### Health Checks

```typescript
// Check if Space is operational
const spaceAcct = await connection.getAccountInfo(spacePda);
const isFrozen = spaceAcct?.data[105] === 1;

if (isFrozen) {
  console.warn('Space is frozen - no new verifications possible');
}
```

### Deployment

#### Mainnet Deployment

**Program ID**: `VrFyyRxPoyWxpABpBXU4YUCCF9p8giDSJUv2oXfDr5q`

**Deployed**: February 2026

**Upgrade Authority**: Grape DAO multisig

#### Testnet

**Devnet Program ID**: Contact team for devnet deployment

### Future Improvements

#### Planned Features

1. **Batch Verification**: Verify multiple identities in one tx
2. **Delegation**: Attestor can delegate to sub-attestors
3. **Self-Verification**: Optional self-sovereign mode
4. **Zero-Knowledge**: ZK proofs for private verification
5. **Cross-Chain**: Verify Solana identities on other chains

#### Research Areas

* Decentralized attestor network
* Machine learning for fraud detection
* Anonymous credentials
* Verifiable credentials standard compliance

***

_For implementation questions, see the_ [_Developer Guide_](./)
