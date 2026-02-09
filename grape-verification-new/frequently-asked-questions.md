# Frequently Asked Questions

## General Questions

#### What is Grape Verification?

Grape Verification is a privacy-preserving identity verification system on Solana that links social media accounts (Discord, Telegram, etc.) to wallet addresses using cryptographic hashes instead of storing actual identifiers.

#### Why use Grape Verification instead of a traditional database?

**Benefits**:

* ✅ Cryptographically verifiable (can't be faked)
* ✅ Decentralized (no single point of failure)
* ✅ Composable (other apps can verify your identity)
* ✅ Privacy-preserving (only hashes stored on-chain)
* ✅ Trustless (no need to trust a central authority)

**Trade-offs**:

* ⚠️ Costs \~0.002 SOL per verification
* ⚠️ Requires blockchain transactions
* ⚠️ More complex integration

#### Is Grape Verification free?

The **verification itself is free** for users. The attestor pays the on-chain transaction fees (\~0.002 SOL). Some communities may require users to hold specific tokens to get verified.

#### Which platforms are supported?

Currently supported:

* ✅ Discord
* ✅ Telegram
* ✅ Email
* 🔜 Twitter (coming soon)
* 🔜 GitHub (planned)

#### Can I link multiple wallets to one account?

**Yes!** You can link unlimited wallets to a single platform identity. Each wallet gets its own Link account on-chain.

#### Can I link the same wallet to multiple platforms?

**Yes!** You can verify your wallet with Discord, Telegram, and Email separately. Each platform gets its own Identity account.

### Privacy & Security

#### What information is stored on-chain?

**On-chain** (public):

* Cryptographic hash of your platform ID
* Cryptographic hash of your wallet address
* Verification timestamp
* Link timestamp

**NOT on-chain**:

* Your actual Discord/Telegram ID
* Your actual wallet address (in account data)
* Your email address
* Any personal information

#### Can someone figure out my identity from the hash?

**No.** SHA-256 hashes are one-way functions. Even with massive computing power, you can't reverse the hash to get the original data.

Additionally, each DAO uses a unique random salt, making rainbow table attacks infeasible.

#### Are my wallet addresses hidden?

**Partially**. Wallet addresses are **not stored in the account data**, only hashes. However, your wallet address **is visible in transaction signatures** when you sign verification or link transactions.

For maximum privacy, use a separate wallet for verification transactions.

#### Who controls my verification?

* **You control**: Which wallets to link, when to unlink
* **Attestor controls**: Verifying your platform identity, revoking verification
* **DAO controls**: Who the attestor is

#### What if the attestor is malicious?

DAOs choose their own attestor. If an attestor misbehaves:

* DAO can replace the attestor
* DAO can revoke fake verifications
* Community can audit all verifications on-chain

Choose communities with trusted attestors.

#### Can my verification be revoked?

**Yes**. The attestor can revoke your verification at any time. Reasons might include:

* Violating community rules
* Account takeover/security concern
* Ban from the platform
* Attestor error

Revoking verification sets `verified = false` but **doesn't delete** your Link accounts. You can still see your linked wallets.

### Technical Questions

#### How does wallet discovery work without a database?

When you link a wallet, you sign a transaction. The bot can later:

1. Look at the Link account creation transaction
2. Extract your wallet from the transaction signers
3. Verify the wallet hash matches
4. Use that wallet to check token balances

This works without any database!

#### What if transaction history is too old?

Solana stores \~2-3 months of transaction history. For very old links, the bot might not find the transaction.

**Solutions**:

* Re-link your wallet (creates new transaction)
* Use multiple wallets (backup)
* Community can add a lightweight cache (breaks "no database" rule)

#### Why do I need to sign a message?

Signing proves you own the private key for that wallet. This prevents someone from claiming they own a wallet they don't control.

The message you sign is:

```
Grape Verification Link Request
platform=discord
wallet=YourWalletAddress
ts=1234567890
```

#### What's the difference between Identity and Link accounts?

* **Identity**: Verifies your platform account (Discord/Telegram)
* **Link**: Connects a specific wallet to your identity

```
Identity (Discord user #123)
├─ Link (Wallet A) 
├─ Link (Wallet B)
└─ Link (Wallet C)
```

#### Can I verify without connecting my wallet?

**No**. Verification requires linking at least one wallet. The whole point is to cryptographically link your social identity to a Solana wallet.

#### What happens if I lose access to my Discord?

Your on-chain verification remains, but:

* You can't link new wallets (need Discord OAuth)
* You can't unlink wallets
* Existing links still work for token gating

Contact the community admin to resolve account issues.

### Usage Questions

#### How do I get Discord roles after verifying?

1. Verify your wallet at verification.governance.so
2. Join the Discord server
3. The bot automatically checks your verification
4. Roles are granted based on:
   * Verification status
   * Token/NFT holdings in linked wallets
   * Community-specific requirements

Roles typically sync every 10-15 minutes.

#### Why don't I have the role yet?

Common reasons:

* **Bot hasn't synced**: Wait 15 minutes or run `/sync` if available
* **Wallet has no tokens**: Link a wallet with the required tokens
* **Verification expired**: Check `/status` to see expiration
* **Bot offline**: Contact server admins
* **Role not configured**: Server may not have token gating set up

#### Can I unlink a wallet?

**Yes**. Connect any wallet linked to your identity, then:

1. Go to verification.governance.so
2. Scroll to "Linked Wallets"
3. Click the unlink icon next to the wallet
4. Sign the unlink message
5. The Link account is closed and rent is refunded

#### Do I need SOL in my wallet?

For verification: **No**. The attestor pays transaction fees.

For future actions:

* Unlinking: \~0.000005 SOL transaction fee
* Self-linking additional wallets: \~0.0008 SOL

#### What if I switch wallets?

Your verification is tied to your platform identity, not your wallet. You can:

* Link new wallets anytime
* Unlink old wallets
* Keep all wallets linked

Roles update automatically based on all linked wallets.

### Discord Bot Questions

#### How does the bot know I have tokens?

1. Bot checks if your Discord ID is verified on-chain
2. Bot fetches all linked wallet hashes
3. Bot finds actual wallet addresses from transaction history
4. Bot checks each wallet's token balance
5. Bot grants roles based on holdings

All data comes from the blockchain!

#### Why is the bot checking tokens slowly?

The bot makes multiple RPC calls per user:

* 1 call: Get Space salt
* 1 call: Check Identity
* 1 call: Get all Links
* N calls: Check token balances for N wallets

For 100 users with 2 wallets each, that's \~400 RPC calls. The bot rate-limits to avoid hitting RPC limits.

#### Can I run my own bot?

**Yes!** The bot is open source. You can:

* Deploy your own instance
* Customize role requirements
* Add custom commands
* Use different RPC providers

Coming soon...

#### Does the bot store my data?

Our reference bot is **database-free** - it only reads from the blockchain. However, communities can:

* Add optional caching for performance
* Keep their own records
* Use different bot implementations

Check with each community about their bot's data practices.

### Developer Questions

#### How do I integrate Grape Verification?

```bash
npm install @grapenpm/grape-verification-registry
```

See the [Developer Guide](https://claude.ai/chat/developer-guide.md) for full integration instructions.

#### What RPC provider should I use?

**Free** (testing only):

* Solana public RPC (rate limited)

**Paid** (production):

* Helius (recommended)
* QuickNode
* Alchemy

For production bots checking 100+ users, you need a paid RPC.

#### Can I verify users programmatically?

**Yes**, if you're the attestor for a Space. See `buildAttestIdentityIx` and `buildLinkWalletIx` in the Developer Guide.

Most developers should use the web interface (verification.governance.so) for user verification and just query on-chain status.

#### How do I check if someone is verified?

```typescript
import { isUserVerified } from '@grapenpm/grape-verification-registry';

const verified = await isUserVerified(connection, discordId, daoId);
```

Full example in [Developer Guide](https://claude.ai/chat/developer-guide.md).

#### Can I use this on Devnet?

**Yes**, but you need to:

1. Deploy the program to Devnet
2. Initialize your own Space
3. Set up your own attestor

Contact us for Devnet deployment assistance.

### Cost & Economics

#### How much does verification cost?

**For users**: Free (attestor pays)

**For attestors**:

* Initialize Space: \~0.0015 SOL (one-time)
* Attest Identity: \~0.0012 SOL per user
* Link Wallet: \~0.0008 SOL per wallet

**Total for 100 users with 1 wallet each**: \~0.2 SOL (≈$20 at $100/SOL)

#### Who pays for the SOL?

The **attestor** (the verification service) pays for:

* Creating Identity accounts
* Creating Link accounts
* All transaction fees

Users don't need any SOL to get verified.

#### Can users reclaim SOL?

When unlinking a wallet, the Link account is closed and rent (\~0.0008 SOL) is refunded to a recipient address (typically the user's wallet or attestor).

#### Is there a subscription fee?

**No**. Verification is a one-time on-chain transaction. Once verified, it remains until revoked.

Some communities may require holding specific tokens to maintain roles, but that's separate from Grape Verification itself.

### Troubleshooting

#### "Space account not found"

The DAO hasn't initialized Grape Verification yet. Contact the community admins to set it up.

#### "Identity not verified"

You haven't completed verification. Go to verification.governance.so and verify your account.

#### "Link exists" but shows 0 wallets

This is a display bug. Click **Refresh** to update the count. If it persists, clear cache and refresh the page.

#### "Wallet does not support signMessage"

Some wallets don't support message signing. Use:

* ✅ Phantom
* ✅ Solflare
* ✅ Backpack
* ❌ Some browser extension wallets

#### "Transaction failed"

Common causes:

* Insufficient SOL for transaction fee
* Network congestion (try again)
* Attestor offline (wait and retry)
* RPC node issues (try different RPC)

#### Roles not updating in Discord

1. Wait 15 minutes for auto-sync
2. Check verification status: `/status`
3. Verify wallet has required tokens
4. Contact server admin if bot is offline

#### Can't unlink wallet

You must be connected with a wallet that's linked to the identity. Connect a linked wallet first, then try unlinking.

### Community & Support

#### How do I set up Grape Verification for my DAO?

1. Decide on a DAO identifier (PublicKey)
2. Generate a random 32-byte salt
3. Set up an attestor service (we can help)
4. Initialize your Space account
5. Integrate verification.governance.so
6. Deploy a Discord bot (optional)

Contact us in Discord for setup assistance.

#### Where can I get help?

* **Documentation**: This GitBook
* **Discord**: [Join Grape Community](https://discord.gg/grape)
* **GitHub**: [Issues & Discussions](https://github.com/Grape-Labs/grape-verification)
* **Twitter**: [@grapeprotocol](https://twitter.com/grapeprotocol)

#### How do I report a bug?

* For web app: Discord or GitHub Issues
* For on-chain program: GitHub Issues only
* For documentation: GitHub Issues

#### Can I contribute?

**Yes!** We welcome:

* Code contributions (PRs)
* Documentation improvements
* Bug reports
* Feature suggestions
* Integration examples

See CONTRIBUTING.md in the GitHub repo.

#### Is there a bounty program?

Security vulnerabilities: Contact us privately first.

Feature development: Check GitHub Issues for "bounty" label.

#### What's the project roadmap?

See [Roadmap](https://claude.ai/chat/README.md#roadmap) in the main docs.

### Advanced Topics

#### Can I run my own attestor?

**Yes**, for your own Space. You need:

* Solana wallet with SOL
* Attestor keypair
* OAuth credentials for platforms
* Server to run the attestor API

See Attestor Setup Guide (coming soon).

#### How do I handle multiple DAOs?

Each DAO has its own Space. A user can be verified in multiple Spaces:

```
User's Discord #123
├─ DAO A Space → Identity A → Link (Wallet 1)
├─ DAO B Space → Identity B → Link (Wallet 1)  
└─ DAO C Space → Identity C → Link (Wallet 2)
```

Same user, different verifications per DAO.

#### Can I build a competitor to verification.governance.so?

**Yes!** The protocol is permissionless. You can:

* Build your own UI
* Run your own attestor
* Offer verification services
* Charge fees if you want

Everything is open source (MIT license).

#### What's the admin multisig?

Program upgrade authority: Grape DAO multisig

Emergency admin: `GScbAQoP73BsUZDXSpe8yLCteUx7MJn1qzWATZapTbWt`

Used only for emergency account recovery.

***

_Still have questions? Join our_ [_Discord_](https://discord.gg/grape) _and ask!_
