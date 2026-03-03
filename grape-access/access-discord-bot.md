# Access Discord Bot

### Overview

The Grape Access Discord Bot assigns and removes Discord roles based on on-chain Grape Access gate checks.

It is designed for serverless deployment on Vercel and integrates with:

* Discord Interactions (slash commands)
* Grape Access + Verification + Reputation programs (Solana)
* Verification callback events (wallet linking + auto-sync)
* Scheduled and queued revalidation jobs

### What The Bot Does

* Maps a `gate_id` to a Discord role per guild.
* Builds verification links for members.
* Stores the latest verified wallet per Discord user and guild.
* Runs gate checks and applies role changes.
* Supports manual identity overrides for hard-to-resolve verification cases.
* Runs both:
  * callback-driven sync after verification
  * periodic cron revalidation

### High-Level Flow

1. Admin runs `/setup-gate` to map a gate and role.
2. Member runs `/verify` and completes verification.
3. External verifier calls `/api/verification/link` with `discordUserId`, `walletPubkey`, and `guildId`.
4. Bot stores the wallet link and syncs mapped gate(s).
5. Bot applies role updates based on pass/fail result.
6. Cron (`/api/cron/revalidate`) keeps memberships fresh over time.

### Slash Commands

| Command                                                                                                        | Purpose                                                 | Default Access                                                     |
| -------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------- | ------------------------------------------------------------------ |
| `/setup-gate gate_id pass_role_id [guild_id] [dao_id] [verification_dao_id] [reputation_dao_id] [fail_action]` | Create/update gate-to-role mapping                      | Administrator or Manage Server                                     |
| `/verify`                                                                                                      | Return verification/access links for configured gates   | Everyone                                                           |
| `/check`                                                                                                       | Run manual check against latest linked wallet           | Administrator                                                      |
| `/debug-identity gate_id`                                                                                      | Debug verification identity/link account resolution     | Administrator                                                      |
| `/link-identity gate_id identity_pda [link_pda]`                                                               | Manually set identity/link override                     | Administrator                                                      |
| `/link-wallet wallet`                                                                                          | Manually link wallet (testing/admin)                    | Administrator                                                      |
| `/reset-me`                                                                                                    | Remove latest wallet link + identity overrides          | Administrator                                                      |
| `/sync-gate gate_id [dry_run]`                                                                                 | Queue full-guild sync and run immediate requester check | Admin/Moderator (Manage Roles, Moderate Members, or Administrator) |

### HTTP Endpoints

#### Discord Interactions

* `POST /api/discord/interactions`
* Alias: `POST /api/interactions`
* Validates Discord Ed25519 signature and processes slash commands.

#### Verification Callback

* `POST/GET /api/verification/link`
* Aliases:
  * `/api/discord/callback`
  * `/api/verification/add`
  * `/api/verification/verify`

Required fields:

* `discordUserId`
* `walletPubkey`
* `guildId` (unless `gateId` maps to exactly one guild)

Optional:

* `gateId`, `verifiedAt`, `source`, `identityPda`, `linkPda`

Security:

* Optional shared secret via `VERIFY_SHARED_SECRET`:
  * `x-verify-secret` header
  * `Authorization: Bearer <secret>`
  * `verify_secret` query param

#### Cron Revalidation

* `GET /api/cron/revalidate`
* Optional `CRON_SECRET` via bearer token
* Processes queued `/sync-gate` jobs and periodic gate revalidation.

#### RPC Diagnostics

* `GET /api/rpc/ping`
* Optional `?endpoint=<rpc_url>`
* Returns both `fetch` and `@solana/web3.js` probe results.

### Storage Model

Primary runtime store is Vercel KV. If KV env vars are missing, bot falls back to in-memory state.

Stored entities:

* Gate mappings (`guildId + gateId`)
* Latest wallet link per (`guildId + discordUserId`)
* Identity overrides (`guildId + gateId + discordUserId`)
* Sync job queue
* Check results history
* Last revalidation run timestamps

Use `KV_KEY_PREFIX` to isolate this bot when sharing KV.

### Environment Variables

Required:

* `DISCORD_APP_ID` (or `DISCORD_CLIENT_ID`)
* `DISCORD_BOT_TOKEN` (or `DISCORD_TOKEN`)
* `DISCORD_PUBLIC_KEY`
* `ACCESS_FRONTEND_BASE_URL`

Recommended:

* `KV_REST_API_URL`
* `KV_REST_API_TOKEN`
* `VERIFY_SHARED_SECRET`
* `CRON_SECRET`
* `RPC_ENDPOINT`

Behavior controls:

* `CHECK_MODE` (`simulate` by default)
* `BASIC_IDENTITY_CHECK_MODE` (`true` by default)
* `DEFAULT_RECHECK_INTERVAL_SEC`
* `MAX_SYNC_JOBS_PER_CRON`
* `MAX_USERS_PER_SYNC`
* `RPC_REQUEST_TIMEOUT_MS`
* `DRY_RUN_SYNC`
* `BOOTSTRAP_GATES_JSON`

### Deployment Notes (Vercel)

* Project uses Vercel Functions (`api/**/*.ts`) with `maxDuration: 60`.
* Built-in cron in `vercel.json` runs daily at `0 0 * * *` on `/api/cron/revalidate`.
* Register slash commands after deploy:

```bash
npm run register-commands
```

### Operational Runbook

#### Initial setup

1. Configure environment variables.
2. Deploy to Vercel.
3. Register slash commands.
4. In Discord, run `/setup-gate ...`.
5. Test end-to-end with `/verify` and callback payload.

#### Recommended production behavior

* Keep `CHECK_MODE=simulate` unless you intentionally need on-chain writes.
* Always protect callback and cron with secrets.
* Use `/api/rpc/ping` when checks time out or fail unexpectedly.

### Troubleshooting

#### No roles are being assigned

* Ensure callback is firing to `/api/verification/link`.
* Confirm `discordUserId`, `walletPubkey`, and `guildId` are present.
* Verify mapping exists for guild (`/setup-gate`).
* Check bot has role-management permissions and role hierarchy is correct.

#### `/check` says no linked wallet

* Callback may not be configured.
* User may have verified identity but wallet link is not in bot KV yet.
* If needed, temporarily use `/link-wallet` for testing.

#### Identity-related custom errors (`6004`, `6005`, `6008`, `6009`)

* Run `/debug-identity gate_id`.
* Add manual override with `/link-identity gate_id identity_pda [link_pda]`.
* Ensure verification DAO resolution is correct (`verification_dao_id` / `dao_id`).

#### Cron not processing sync jobs

* Confirm `GET /api/cron/revalidate` is reachable.
* If `CRON_SECRET` is set, ensure valid bearer token is provided.
* Check `MAX_SYNC_JOBS_PER_CRON` and queue volume.

### Example Callback Payload

```json
{
  "discordUserId": "123456789012345678",
  "walletPubkey": "YourWalletPubkeyHere",
  "guildId": "987654321098765432",
  "gateId": "your-gate-id",
  "verifiedAt": "2026-03-03T12:00:00.000Z",
  "source": "grape-verification",
  "identityPda": "OptionalIdentityPda",
  "linkPda": "OptionalLinkPda"
}
```

### Program IDs (Current Config)

* Access: `GPASSzQQF1H8cdj5pUwFkeYEE4VdMQtCrYtUaMXvPz48`
* Verification: `VrFyyRxPoyWxpABpBXU4YUCCF9p8giDSJUv2oXfDr5q`
* Reputation: `V1NE6WCWJPRiVFq5DtaN8p87M9DmmUd2zQuVbvLgQwX`
