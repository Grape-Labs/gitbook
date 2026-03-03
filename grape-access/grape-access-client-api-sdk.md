# Grape Access Client (API/SDK)

`@grapenpm/grape-access-sdk` is a TypeScript SDK for integrating the Grape Access Protocol on Solana.

It helps you:

* Create access spaces ("gates") with programmable criteria.
* Check whether a wallet/user passes those criteria.
* Store and fetch check records on-chain.
* Compose access checks into your own transactions.
* Derive PDAs for Grape Access, Vine Reputation, and Grape Verification programs.

Program ID (Grape Access): `GPASSzQQF1H8cdj5pUwFkeYEE4VdMQtCrYtUaMXvPz48`

### Install

```bash
npm install @grapenpm/grape-access-sdk
npm install @coral-xyz/anchor @solana/web3.js
```

### Quick Start

```ts
import { AnchorProvider } from "@coral-xyz/anchor";
import { Keypair } from "@solana/web3.js";
import {
  GrapeAccessClient,
  AccessCriteriaFactory,
  AccessTypeFactory,
  VerificationPlatform,
} from "@grapenpm/grape-access-sdk";

const provider = AnchorProvider.env();
const client = new GrapeAccessClient(provider);

// You control this key and use it as the deterministic access identifier.
const accessId = Keypair.generate().publicKey;

// 1) Create an access space
const createResult = await client.initializeAccess({
  accessId,
  criteria: AccessCriteriaFactory.verifiedIdentity({
    grapeSpace: grapeSpacePda,
    platforms: [VerificationPlatform.Discord],
  }),
  accessType: AccessTypeFactory.reusable(),
  metadataUri: "https://example.com/access/1.json",
});

// 2) Check a user
const checkTx = await client.checkAccess({
  accessId,
  user: userWallet,
  identityAccount: userIdentityPda, // required for verifiedIdentity
  storeRecord: true,
});

// 3) Optional readbacks
const access = await client.fetchAccess(accessId);
const record = await client.fetchAccessCheckRecord(accessId, userWallet);
```

### What The SDK Wraps

The SDK wraps the on-chain instruction set for:

* `initializeAccess`
* `checkAccess`
* `updateAccessCriteria`
* `updateMetadataUri`
* `setAccessActive`
* `setAccessAuthority`
* `closeAccess`
* `closeCheckRecord`

It also exposes helper methods for account fetches and instruction building.

### Core Client Methods

Read/write methods:

* `initializeAccess`
* `checkAccess`
* `simulateCheckAccess` (returns `boolean`)
* `updateAccessCriteria`
* `updateMetadataUri`
* `setAccessActive`
* `setAccessAuthority`
* `closeAccess`
* `closeAccessCheckRecord`

Fetch methods:

* `fetchAccess`
* `fetchAccessCheckRecord`
* `fetchAccessesByAuthority`

Instruction composition:

* `buildCheckAccessInstruction`
* `buildAccessTransaction`

### Access Criteria

Create criteria values with `AccessCriteriaFactory`:

* `minReputation({ vineConfig, minPoints, season })`
* `verifiedIdentity({ grapeSpace, platforms })`
* `verifiedWithWallet({ grapeSpace, platforms })`
* `combined({ vineConfig, minPoints, season, grapeSpace, platforms, requireWalletLink })`
* `timeLockedReputation({ vineConfig, minPoints, season, minHoldDurationSeconds })`
* `multiDao({ requiredAccessSpaces, requireAll })`
* `tokenHolding({ mint, minAmount, checkAta })`
* `nftCollection({ collectionMint, minCount })`
* `customProgram({ programId, instructionDataHash })`

Platform enum values:

* `VerificationPlatform.Discord`
* `VerificationPlatform.Telegram`
* `VerificationPlatform.Twitter`
* `VerificationPlatform.Email`

#### Accounts Needed During `checkAccess`

Pass only what your criteria requires:

* Reputation criteria: `reputationAccount`
* Identity criteria: `identityAccount`
* Wallet-link criteria: `linkAccount`
* Token-holding criteria: `tokenAccount`
* Criteria relying on `remaining_accounts` (for example `multiDao`, `nftCollection`, `customProgram`): `remainingAccounts`

For optional persistent pass/fail storage, set `storeRecord: true`.

### Access Types

Use `AccessTypeFactory`:

* `singleUse()`
* `reusable()`
* `timeLimited(durationSeconds)`
* `subscription(intervalSeconds)`

### PDA Helpers

The SDK provides deterministic PDA helpers:

```ts
import {
  findAccessPda,
  findAccessCheckRecordPda,
  findVineConfigPda,
  findVineReputationPda,
  findGrapeSpacePda,
  findGrapeIdentityPda,
  findGrapeLinkPda,
} from "@grapenpm/grape-access-sdk";

const [accessPda] = await findAccessPda(accessId);
const [recordPda] = await findAccessCheckRecordPda(accessPda, userWallet);
```

Use these when you need to:

* Precompute accounts for UI/backend flows.
* Provide external accounts to `checkAccess`.
* Validate account wiring in integration tests.

### Authority and Signer Behavior

`initializeAccess` defaults authority to `provider.wallet.publicKey`.

If authority is a different key, pass both:

* `authority`
* `authoritySigner`

Example:

```ts
await client.initializeAccess({
  accessId,
  criteria,
  accessType,
  authority: externalAuthority.publicKey,
  authoritySigner: externalAuthority,
});
```

### Backward Compatibility (`Gate*` Naming)

The SDK includes aliases so older integrations continue to work:

* `GpassClient` (alias of `GrapeAccessClient`)
* `GateTypeFactory` (alias of `AccessTypeFactory`)
* `GateCriteriaFactory` (alias of `AccessCriteriaFactory`)
* `findGatePda`, `findCheckRecordPda`
* Wrapper methods such as `initializeGate`, `checkGate`, `fetchGate`

For new integrations, prefer the `Access*` naming.

### Recommended Integration Pattern

1. Derive or load all required external PDAs/accounts (Vine/Grape/etc).
2. Build criteria with the relevant factory function.
3. Create one access space per permission boundary in your product.
4. Use `simulateCheckAccess` for non-blocking UX previews.
5. Use `checkAccess` before sensitive instructions and optionally store records.
6. For atomic flows, compose with `buildCheckAccessInstruction`.

### Exports

From the package root, you get:

* Client classes
* IDL and protocol types
* PDA helper functions
* Constants for all three program IDs
* Criteria/type factories and associated TypeScript types
