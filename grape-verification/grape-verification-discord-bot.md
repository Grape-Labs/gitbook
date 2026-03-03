# Grape Verification Discord Bot

## Overview

The Grape Verification Discord Bot connects Discord users to on-chain identity records in the Grape Verification Registry (GVR) on Solana.

It provides a Discord-first verification flow:

1. User runs `/gv verify` and opens the web verification portal.
2. User completes verification on the portal.
3. User runs `/gv check` to confirm on-chain verification and sync Discord role access.

### Core Features

* Secure Discord interaction handling (Ed25519 signature verification).
* DAO-aware verification per server (guild).
* Per-guild configuration via `/gv setup`.
* Automatic verified-role sync on `/gv check`.
* Admin tools for on-chain GVR operations (space init, attest, revoke, wallet link/unlink).

### Slash Commands

#### User Commands

* `/gv verify`\
  Opens the verification portal with Discord user and guild context.
* `/gv check`\
  Checks if the user has a verified identity in GVR for the configured DAO.
* `/gv ping`\
  Health check command.

#### Admin Command

* `/gv setup dao:<pubkey> verified_role:<role?>`\
  Stores server-specific DAO and optional verified role.\
  Requires **Manage Server** or **Administrator** permission.

#### Advanced Admin Subcommands (`/gv admin ...`)

* `status` — Shows derived PDAs and current on-chain status.
* `space-init` — Initializes DAO space account (requires signer key).
* `attest` — Attests an identity.
* `revoke` — Revokes an identity.
* `link` — Links wallet to identity (attestor-driven).
* `link-self` — Links wallet requiring wallet signer.
* `unlink` — Unlinks a wallet from identity.

### Architecture

* `pages/api/interactions.js`\
  Primary Discord interaction webhook endpoint.
* `lib/discord/slash-commands.js`\
  Command schema, dispatch, DAO resolution, role sync, and Solana/GVR logic.
* `scripts/register-commands.js`\
  Registers slash commands with Discord API.
* `pages/api/discord.js`\
  Separate endpoint for a `points` command flow (independent from `/gv`).

### Configuration

#### Required

* `DISCORD_PUBLIC_KEY`
* `DISCORD_BOT_TOKEN`
* `DISCORD_APPLICATION_ID` (or `DISCORD_APP_ID`)

#### Strongly Recommended

* `DISCORD_TEST_GUILD_ID` (for fast guild-scoped command registration during development)
* `SOLANA_RPC_URL` (or compatible RPC env variant)
* `VERIFICATION_URL` (defaults to `https://verification.governance.so`)

#### DAO / Role Resolution

* `GVR_DAO` (default DAO)
* `GVR_GUILD_DAO_MAP` (JSON map: guild ID -> DAO pubkey)
* `GVR_VERIFIED_ROLE_ID` (default role ID)
* `GVR_GUILD_ROLE_MAP` (JSON map: guild ID -> role ID)
* `GVR_REMOVE_ROLE_ON_UNVERIFIED` (`true` by default)

#### On-chain Admin Operations

* `BOT_FEE_PAYER_SECRET_KEY` (JSON array, base58, or base64)
* `BOT_WALLET_SIGNER_SECRET_KEY` (only for `link-self`)

#### Optional Persistent Guild Config

If Vercel KV is configured, `/gv setup` persists per-guild DAO/role in KV and overrides static defaults.

### Operational Notes

* All command responses are ephemeral by default.
* Role assignment/removal is attempted during `/gv check`.
* If DAO space is not initialized, `/gv check` prompts admin action.
* Command registration supports both global and guild-scoped modes.

### Quick Start

1. Set environment variables.
2. Run command registration:
   * `yarn register:commands`
3. Configure Discord Interactions endpoint to your deployed API route:
   * `POST /api/interactions`
4. In a server, run:
   * `/gv setup`
   * `/gv verify`
   * `/gv check`

### Security Notes

* Never expose bot tokens or private keys in documentation.
* Rotate credentials immediately if leaked.
* Limit admin command usage to trusted roles.
