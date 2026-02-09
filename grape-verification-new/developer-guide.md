# Developer Guide

## Developer Guide

### Integrating Grape Verification

This guide covers how to integrate Grape Verification into your application, Discord bot, or DAO tooling.

### Installation

#### NPM Package

```bash
npm install @grapenpm/grape-verification-registry
```

or

```bash
yarn add @grapenpm/grape-verification-registry
pnpm add @grapenpm/grape-verification-registry
```

#### Dependencies

```json
{
  "dependencies": {
    "@solana/web3.js": "^1.95.0",
    "@noble/hashes": "^1.3.3"
  }
}
```

### Quick Example

```typescript
import { Connection, PublicKey } from '@solana/web3.js';
import {
  deriveSpacePda,
  deriveIdentityPda,
  fetchLinkedWallets,
  identityHash,
  TAG_DISCORD,
  VerificationPlatform,
} from '@grapenpm/grape-verification-registry';

const connection = new Connection('https://api.mainnet-beta.solana.com');
const DAO_ID = new PublicKey('YourDAOPubkeyHere');

// Check if a Discord user is verified
async function isUserVerified(discordId: string): Promise<boolean> {
  // 1. Derive Space PDA
  const [spacePda] = deriveSpacePda(DAO_ID);
  
  // 2. Get space account to read salt
  const spaceAcct = await connection.getAccountInfo(spacePda);
  if (!spaceAcct) return false;
  
  // 3. Parse salt (offset 107, 32 bytes)
  const SALT_OFFSET = 107;
  const salt = spaceAcct.data.slice(SALT_OFFSET, SALT_OFFSET + 32);
  
  // 4. Derive Identity PDA
  const idHash = identityHash(salt, TAG_DISCORD, discordId);
  const [identityPda] = deriveIdentityPda(
    spacePda,
    VerificationPlatform.Discord,
    idHash
  );
  
  // 5. Check if identity exists and is verified
  const identityAcct = await connection.getAccountInfo(identityPda);
  if (!identityAcct) return false;
  
  // Parse verified flag (offset 74)
  return identityAcct.data[74] === 1;
}

// Usage
const verified = await isUserVerified('123456789');
console.log(`User verified: ${verified}`);
```

### Core Concepts

#### Program ID

```typescript
import { PROGRAM_ID } from '@grapenpm/grape-verification-registry';

console.log(PROGRAM_ID.toBase58());
// Output: VrFyyRxPoyWxpABpBXU4YUCCF9p8giDSJUv2oXfDr5q
```

#### Account Hierarchy

```
Space (per DAO)
  └── Identity (per platform user)
        └── Link (per wallet)
```

#### PDA Derivation

```typescript
import { 
  deriveSpacePda, 
  deriveIdentityPda, 
  deriveLinkPda 
} from '@grapenpm/grape-verification-registry';

// Space PDA
const [spacePda, spaceBump] = deriveSpacePda(daoId);
// seeds: ["space", dao_id]

// Identity PDA
const [identityPda, identityBump] = deriveIdentityPda(
  spacePda,
  platformSeed,  // 0=Discord, 1=Telegram, 2=Twitter, 3=Email
  idHash         // 32-byte hash
);
// seeds: ["identity", space, platform_seed, id_hash]

// Link PDA
const [linkPda, linkBump] = deriveLinkPda(
  identityPda,
  walletHash  // 32-byte hash
);
// seeds: ["link", identity, wallet_hash]
```

### Hashing Functions

#### Identity Hash

```typescript
import { identityHash, TAG_DISCORD } from '@grapenpm/grape-verification-registry';

const discordId = '123456789';
const salt = new Uint8Array(32); // from Space account

const hash = identityHash(
  salt,
  TAG_DISCORD,  // or TAG_TELEGRAM, TAG_TWITTER, TAG_EMAIL
  discordId
);

// hash = SHA256(salt || "discord" || "123456789")
```

#### Wallet Hash

```typescript
import { walletHash } from '@grapenpm/grape-verification-registry';

const wallet = new PublicKey('KirkNf6VGMgc8dcbp5Zx3EKbDzN6goyTBMKN9hxSnBT');
const salt = new Uint8Array(32); // from Space account

const hash = walletHash(salt, wallet);

// hash = SHA256(salt || "wallet" || wallet_pubkey_bytes)
```

### Reading On-Chain Data

#### Check Verification Status

```typescript
async function getVerificationStatus(discordId: string) {
  const [spacePda] = deriveSpacePda(DAO_ID);
  const spaceAcct = await connection.getAccountInfo(spacePda);
  
  if (!spaceAcct) {
    throw new Error('Space not initialized');
  }
  
  // Parse salt
  const salt = spaceAcct.data.slice(107, 139);
  
  // Derive identity
  const idHash = identityHash(salt, TAG_DISCORD, discordId);
  const [identityPda] = deriveIdentityPda(
    spacePda,
    VerificationPlatform.Discord,
    idHash
  );
  
  const identityAcct = await connection.getAccountInfo(identityPda);
  
  if (!identityAcct) {
    return {
      verified: false,
      exists: false,
    };
  }
  
  // Parse identity account
  const data = identityAcct.data;
  const verified = data[74] === 1;
  const verifiedAt = data.readBigInt64LE(75);
  const expiresAt = data.readBigInt64LE(83);
  
  // Check expiration
  const now = BigInt(Math.floor(Date.now() / 1000));
  const expired = expiresAt > 0n && now > expiresAt;
  
  return {
    verified: verified && !expired,
    exists: true,
    verifiedAt: Number(verifiedAt),
    expiresAt: Number(expiresAt),
  };
}
```

#### Fetch Linked Wallets

```typescript
import { fetchLinkedWallets } from '@grapenpm/grape-verification-registry';

const linkedWallets = await fetchLinkedWallets(
  connection,
  identityPda,
  currentWalletHash  // optional: mark current wallet
);

console.log(`User has ${linkedWallets.length} linked wallets`);

linkedWallets.forEach(wallet => {
  console.log({
    linkPda: wallet.pubkey.toBase58(),
    walletHash: wallet.walletHashHex,
    linkedAt: new Date(wallet.linkedAt * 1000),
    isCurrent: wallet.isCurrentWallet,
  });
});
```

### Building Transactions

\{% hint style="warning" %\} Most applications should use the web interface at verification.governance.so for user verification. These instructions are for advanced use cases like building custom attestor services. \{% endhint %\}

#### Initialize Space (DAO Admins Only)

```typescript
import { 
  buildInitializeSpaceIx 
} from '@grapenpm/grape-verification-registry';

// Generate random salt
const salt = crypto.randomBytes(32);

const { ix, spaceAcct } = buildInitializeSpaceIx({
  daoId: DAO_ID,
  salt,
  authority: authorityPubkey,
  payer: payerPubkey,
});

const tx = new Transaction().add(ix);
// ... sign and send
```

#### Attest Identity (Attestor Only)

```typescript
import { buildAttestIdentityIx } from '@grapenpm/grape-verification-registry';

const { ix } = buildAttestIdentityIx({
  daoId: DAO_ID,
  platform: VerificationPlatform.Discord,
  platformSeed: 0, // Must match platform enum
  idHash,
  expiresAt: 0n, // 0 = never expires
  attestor: attestorPubkey,
  payer: payerPubkey,
});

const tx = new Transaction().add(ix);
// ... sign with attestor keypair
```

#### Link Wallet (Attestor Only)

```typescript
import { buildLinkWalletIx } from '@grapenpm/grape-verification-registry';

const { ix } = buildLinkWalletIx({
  daoId: DAO_ID,
  platformSeed: 0,
  idHash,
  wallet: userWalletPubkey,
  walletHash,
  attestor: attestorPubkey,
  payer: payerPubkey,
});

const tx = new Transaction().add(ix);
// ... sign with attestor keypair
```

### Discord Bot Integration

#### Basic Verification Check

```typescript
async function grantVerifiedRole(discordUserId: string) {
  const verified = await isUserVerified(discordUserId);
  
  if (verified) {
    // Grant Discord role
    await member.roles.add(verifiedRoleId);
  }
}
```

#### Token-Gated Roles

```typescript
async function checkTokenGate(
  discordUserId: string,
  tokenMint: PublicKey,
  minAmount: number
): Promise<boolean> {
  // 1. Get verification status
  const status = await getVerificationStatus(discordUserId);
  if (!status.verified) return false;
  
  // 2. Get linked wallets
  const [spacePda] = deriveSpacePda(DAO_ID);
  const spaceAcct = await connection.getAccountInfo(spacePda);
  const salt = spaceAcct.data.slice(107, 139);
  
  const idHash = identityHash(salt, TAG_DISCORD, discordUserId);
  const [identityPda] = deriveIdentityPda(
    spacePda,
    VerificationPlatform.Discord,
    idHash
  );
  
  const linkedWallets = await fetchLinkedWallets(connection, identityPda);
  
  // 3. Check each wallet for tokens
  // Note: You need to resolve wallet hashes to pubkeys
  // (see Discord Bot guide for wallet discovery methods)
  
  for (const link of linkedWallets) {
    // Discover actual wallet from transaction history
    const wallet = await findWalletFromLinkAccount(link.pubkey);
    if (!wallet) continue;
    
    // Check token balance
    const balance = await getTokenBalance(connection, wallet, tokenMint);
    if (balance >= minAmount) {
      return true;
    }
  }
  
  return false;
}
```

### Account Layouts

#### Space Account (144 bytes)

```rust
pub struct GrapeVerificationSpace {
    pub version: u8,           // 1 byte
    pub dao_id: Pubkey,        // 32 bytes
    pub authority: Pubkey,     // 32 bytes
    pub attestor: Pubkey,      // 32 bytes
    pub is_frozen: bool,       // 1 byte
    pub bump: u8,              // 1 byte
    pub salt: [u8; 32],        // 32 bytes
    pub _padding: [u8; 5],     // 5 bytes
}
```

**Total**: 8 (discriminator) + 136 = 144 bytes

**Offsets**:

```typescript
const SALT_OFFSET = 8 + 1 + 32 + 32 + 32 + 1 + 1; // 107
const salt = data.slice(SALT_OFFSET, SALT_OFFSET + 32);
```

#### Identity Account (128 bytes)

```rust
pub struct GrapeVerificationIdentity {
    pub version: u8,           // 1 byte
    pub space: Pubkey,         // 32 bytes
    pub platform: u8,          // 1 byte
    pub id_hash: [u8; 32],     // 32 bytes
    pub verified: bool,        // 1 byte
    pub verified_at: i64,      // 8 bytes
    pub expires_at: i64,       // 8 bytes
    pub attested_by: Pubkey,   // 32 bytes
    pub bump: u8,              // 1 byte
    pub _padding: [u8; 4],     // 4 bytes
}
```

**Total**: 8 (discriminator) + 120 = 128 bytes

**Offsets**:

```typescript
const verified = data[8 + 1 + 32 + 1 + 32];           // offset 74
const verifiedAt = data.readBigInt64LE(75);           // offset 75
const expiresAt = data.readBigInt64LE(83);            // offset 83
```

#### Link Account (88 bytes)

```rust
pub struct GrapeVerificationLink {
    pub version: u8,           // 1 byte
    pub identity: Pubkey,      // 32 bytes
    pub wallet_hash: [u8; 32], // 32 bytes
    pub linked_at: i64,        // 8 bytes
    pub bump: u8,              // 1 byte
    pub _padding: [u8; 6],     // 6 bytes
}
```

**Total**: 8 (discriminator) + 80 = 88 bytes

**Parsing**:

```typescript
import { parseLink } from '@grapenpm/grape-verification-registry';

const linkAcct = await connection.getAccountInfo(linkPda);
const parsed = parseLink(linkAcct.data);

console.log({
  identity: parsed.identity.toBase58(),
  walletHash: parsed.walletHash,
  linkedAt: parsed.linkedAt,
});
```

### Error Handling

```typescript
import { GrapeVerifyError } from '@grapenpm/grape-verification-registry';

try {
  const verified = await isUserVerified(discordId);
} catch (error) {
  if (error.message.includes('Space not found')) {
    console.error('DAO has not initialized Grape Verification');
  } else if (error.message.includes('Identity not found')) {
    console.log('User not verified yet');
  } else {
    console.error('Unexpected error:', error);
  }
}
```

### Best Practices

#### Caching

```typescript
// Cache verification status for 15 minutes
const cache = new Map<string, { verified: boolean; expires: number }>();

async function getCachedVerification(discordId: string): Promise<boolean> {
  const cached = cache.get(discordId);
  const now = Date.now();
  
  if (cached && cached.expires > now) {
    return cached.verified;
  }
  
  const verified = await isUserVerified(discordId);
  
  cache.set(discordId, {
    verified,
    expires: now + 15 * 60 * 1000, // 15 min
  });
  
  return verified;
}
```

#### Batch Checks

```typescript
import { Connection } from '@solana/web3.js';

async function checkMultipleUsers(
  discordIds: string[]
): Promise<Map<string, boolean>> {
  const results = new Map();
  
  // Derive all PDAs
  const [spacePda] = deriveSpacePda(DAO_ID);
  const spaceAcct = await connection.getAccountInfo(spacePda);
  const salt = spaceAcct.data.slice(107, 139);
  
  const identityPdas = discordIds.map(id => {
    const idHash = identityHash(salt, TAG_DISCORD, id);
    const [pda] = deriveIdentityPda(
      spacePda,
      VerificationPlatform.Discord,
      idHash
    );
    return pda;
  });
  
  // Batch fetch (max 100 per call)
  const accounts = await connection.getMultipleAccountsInfo(identityPdas);
  
  accounts.forEach((account, i) => {
    const verified = account ? account.data[74] === 1 : false;
    results.set(discordIds[i], verified);
  });
  
  return results;
}
```

#### RPC Recommendations

For production use:

* **Helius**: Recommended, reliable, generous rate limits
* **QuickNode**: Good performance, professional support
* **Alchemy**: Solana support, enterprise grade

Avoid free public RPCs for production applications.

### Testing

```typescript
import { Connection, Keypair } from '@solana/web3.js';

// Use devnet for testing
const connection = new Connection(
  'https://api.devnet.solana.com',
  'confirmed'
);

// Deploy program to devnet first
const DEVNET_PROGRAM_ID = new PublicKey('YourDevnetProgramId');

// Test verification flow
async function testVerification() {
  const testDiscordId = '999999999';
  const verified = await isUserVerified(testDiscordId);
  console.assert(!verified, 'Should not be verified initially');
  
  // ... test attestation ...
  
  const verifiedAfter = await isUserVerified(testDiscordId);
  console.assert(verifiedAfter, 'Should be verified after attestation');
}
```

### Examples Repository

Full working examples available at:

* [Discord Bot (Database)](https://github.com/Grape-Labs/verification-discord-bot)
* [Discord Bot (On-Chain Only)](https://github.com/Grape-Labs/verification-discord-bot-onchain)
* [Web Interface](https://github.com/Grape-Labs/verification-web)

### Support

Need help integrating?

* Join our [Discord](https://discord.gg/grape)
* Read the [FAQ](frequently-asked-questions.md)

***

_Happy building! 🍇_
