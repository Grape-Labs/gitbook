# Getting Started

## Grape Verification

### Privacy-Preserving On-Chain Identity Verification

Grape Verification is a decentralized identity verification system on Solana that enables privacy-preserving links between social platform accounts (Discord, Telegram, Twitter, Email) and Solana wallet addresses.

**Live on Mainnet**: [https://verification.governance.so](https://verification.governance.so/)

### What is Grape Verification?

Grape Verification allows users to cryptographically link their social media identities to Solana wallets **without exposing sensitive information on-chain**. Instead of storing actual Discord IDs or wallet addresses, the system stores only cryptographic hashes, ensuring privacy while maintaining verifiability.

#### Key Features

* 🔐 **Privacy-First**: Only cryptographic hashes stored on-chain
* 🌐 **Multi-Platform**: Discord, Telegram, Twitter, Email support
* 💼 **Multi-Wallet**: Link multiple wallets to a single identity
* ✅ **Attestor-Verified**: Trusted verification through authorized attestors
* 🔗 **Composable**: Other applications can verify users on-chain
* ⚡ **Decentralized**: No central database required

### How It Works

```
┌──────────────┐
│ Discord User │
└──────┬───────┘
       │
       │ 1. Connects wallet
       ↓
┌─────────────────────┐
│   Web Interface     │
│ verification.gov... │
└──────┬──────────────┘
       │
       │ 2. Signs consent message
       ↓
┌─────────────────────┐
│   Attestor API      │
│ Verifies identity   │
└──────┬──────────────┘
       │
       │ 3. Submits transaction
       ↓
┌─────────────────────┐
│ Solana Blockchain   │
│ ✅ Identity created  │
│ ✅ Link created      │
└─────────────────────┘
```

#### Privacy Model

**On-Chain** (Public):

* `identity_hash` = SHA256(salt + platform + user\_id)
* `wallet_hash` = SHA256(salt + "wallet" + wallet\_pubkey)

**Off-Chain** (Private):

* Actual Discord ID
* Actual wallet addresses
* Platform credentials

This ensures that while verification is publicly verifiable, **no personally identifiable information is exposed on the blockchain**.

### Use Cases

#### 🎮 Discord Token Gating

Grant Discord roles based on on-chain token/NFT holdings:

* Verify users own specific NFTs
* Require minimum token balances
* Proof of DAO membership
* Exclusive channel access

#### 🏛️ DAO Governance

* Sybil-resistant voting (one identity, multiple wallets)
* Multi-wallet voting power aggregation
* Verified community member status
* Platform-agnostic identity

#### 🎁 Airdrops & Rewards

* Fair distribution to verified identities
* Prevent multi-account farming
* Reward active community members
* Cross-platform engagement tracking

#### 🛡️ Access Control

* Proof of identity without doxxing
* Gated content based on holdings
* VIP access for verified holders
* Anonymous but verified participation

### Quick Start

#### For Users

1. Visit [verification.governance.so](https://verification.governance.so/)
2. Select your platform (Discord, Telegram, etc.)
3. Connect your Solana wallet
4. Sign the verification message
5. Your wallet is now linked on-chain! ✅

#### For Developers

```bash
npm install @grapenpm/grape-verification-registry
```

```typescript
import {
  deriveIdentityPda,
  fetchLinkedWallets,
  checkVerification
} from '@grapenpm/grape-verification-registry';

// Check if a Discord user is verified
const isVerified = await checkVerification(connection, discordId);
```

See [Developer Guide](developer-guide.md) for full integration details.

### Architecture

Grape Verification consists of three on-chain accounts:

#### Space Account

Per-DAO configuration containing:

* DAO identifier
* Random salt (32 bytes)
* Authorized attestor
* Frozen status

#### Identity Account

Per-user verification status:

* Hashed platform identifier
* Verification status
* Verification timestamp
* Expiration (optional)

#### Link Account

Per-wallet connection:

* Identity reference
* Hashed wallet address
* Link timestamp

### Why Grape Verification?

#### vs. Traditional Database Verification

| Feature                     | Traditional        | Grape Verification  |
| --------------------------- | ------------------ | ------------------- |
| **Data Storage**            | Centralized DB     | On-chain            |
| **Verification**            | Trust the service  | Cryptographic proof |
| **Privacy**                 | Full data exposure | Hash-based privacy  |
| **Composability**           | Siloed             | Anyone can verify   |
| **Censorship**              | Possible           | Resistant           |
| **Single Point of Failure** | Yes                | No                  |

#### vs. Other On-Chain Identity Solutions

* **No PII on-chain**: Unlike systems that store plaintext identifiers
* **Multi-wallet support**: Link unlimited wallets to one identity
* **Platform agnostic**: Works with any social platform
* **Lightweight**: Minimal on-chain footprint
* **Permissioned attestation**: Prevents spam and ensures quality

### Security Model

#### Cryptographic Guarantees

1. **Hash Collisions**: SHA-256 makes it computationally infeasible to find two inputs with the same hash
2. **Salt Randomization**: Per-DAO salt prevents rainbow table attacks
3. **Signature Verification**: Users must sign consent messages with their wallets
4. **Attestor Authorization**: Only authorized attestors can create verifications

#### Trust Assumptions

* **Attestor Honesty**: The attestor must correctly verify platform identities
* **Platform OAuth**: Discord/Telegram/etc. OAuth flows are secure
* **RPC Reliability**: Blockchain state is correctly reported

#### Privacy Considerations

**What's Hidden**:

* Actual Discord/Telegram user IDs
* Actual wallet addresses (in on-chain data)
* Personal information from platforms

**What's Visible**:

* Hashed identities
* Hashed wallets
* That a verification exists
* Verification timestamps

**Note**: Wallet addresses are still visible in transaction signatures, but not stored in account data.

### Roadmap

#### Current (V1)

* \[x] Discord verification
* \[x] Telegram verification
* \[x] Email verification
* \[x] Multi-wallet support
* \[x] Web interface
* \[x] NPM package

#### Coming Soon (V2)

* \[ ] Twitter verification
* \[ ] GitHub verification
* \[ ] Self-sovereign identity options
* \[ ] Delegated attestation
* \[ ] Batch operations
* \[ ] Mobile app

#### Future (V3)

* \[ ] Cross-chain verification
* \[ ] Decentralized attestor network
* \[ ] Zero-knowledge proofs
* \[ ] Anonymous credentials

### Support

* **Website**: [verification.governance.so](https://verification.governance.so/)
* **Documentation**: This GitBook
* **Program ID**: `VrFyyRxPoyWxpABpBXU4YUCCF9p8giDSJUv2oXfDr5q`
* **GitHub**: [github.com/Grape-Labs/grape-verification](https://github.com/Grape-Labs/grape-verification)
* **Discord**: [Join Grape Community](https://discord.gg/grape)

### License

MIT License - See LICENSE file for details

***

_Built with 💜 by the Grape DAO_
