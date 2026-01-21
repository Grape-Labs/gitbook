# OG Discord Bot

The OG Discord Bot enables DAO participation tracking directly inside Discord.

It allows communities to check in with Solana wallets, award on-chain participation points, and link a Discord server to a Vine Reputation Space (DAO).

This guide explains how to configure the bot and how each slash command works.

***

### 🔐 Permissions & Requirements

* The bot must be installed with the applications.commands scope
* Commands must be used inside a Discord server (not DMs)
* Participation commands must be run inside the participation thread
* Some commands require the user to be in an active voice/stage call
* Each server can be linked to one Vine Reputation Space (DAO)

***

### 🧩 Server Setup

#### /setspace

Set the Vine Reputation Space (DAO) for this Discord server

This links your Discord server to a Vine DAO. All participation points will be awarded inside this Space.

```
/setspace space:<DAO_SPACE_PUBLIC_KEY>
```

Options

* space (required): Vine Space / DAO public key

Notes

* Usually restricted to admins or moderators
* A DAO can only be assigned to one server at a time
* Can be changed later if needed

```
/getspace
```

***

Displays:

* DAO public key
* Link to the Vine Space on governance.so

***

### 🟢 Starting a Participation Session

#### /startparticipation

Start a new participation day inside a thread

This command posts:

* The current date
* Instructions for checking in
* An internal marker used by the bot to track eligibility

```
/startparticipation
```

Optional

```
/startparticipation note:<agenda, links, or context>
```

Important

* Must be run once per day per thread
* Participants can only check in _after_ this command is used

***

### 👤 Participant Check-In

#### /checkin

Check in with a Solana wallet

```
/checkin wallet:<SOLANA_WALLET>
```

Options

* wallet (required): Your Solana wallet address
* fix (optional): Set to true if you need to correct your wallet _before awards_

Behavior

* The first wallet per user per day is used
* Wallets are tied to the Discord user
* Check-ins are only valid for the current day

Fixing a mistake

```
/checkin wallet:<CORRECT_WALLET> fix:true
```

This only works before participation is awarded.

***

#### /checkinwithlastwallet

Check in using your previously used wallet

```
/checkinwithlastwallet
```

Behavior

* Reuses the most recent wallet you previously checked in with
* Only works if your earlier check-in is still visible in thread history
* If you already checked in today, the bot will show your recorded wallet instead

Recommended for

* Regular contributors
* Avoiding copy/paste errors

***

### 🏆 Awarding Participation

#### /award\_participation

Award 1 participation point to each eligible participant

```
/award_participation
```

What this does

* Collects one wallet per Discord user
* Applies any valid wallet fixes
* Awards 1 participation point per wallet on-chain
* Locks the day so awards cannot be repeated

Output

* A confirmation message with a daily lock marker
* A detailed summary including:
  * Total eligible wallets
  * Success and failure counts
  * Sample transaction links
  * A link to the Vine Space

Important Rules

* Can only be run once per day per thread
* Can only be run after /startparticipation
* Check-ins close immediately after awards are issued

***

### 🔒 Safeguards & Design Notes

*   Voice gating (optional):

    Commands can require users to be in a voice or stage channel
*   Rate-limit safe:

    The bot only reads a limited number of thread messages
*   Wallet safety:

    Wallets are validated before use
*   Transparent auditing:

    All awards are posted publicly in the thread

***

### Typical Flow

1. Admin runs /setspace (once per server)
2. Moderator runs /startparticipation
3. Users run /checkin or /checkinwithlastwallet
4. Moderator runs /award\_participation
5. Participation points are awarded on-chain 🎉

***

### Tips

* Encourage users to use /checkinwithlastwallet to reduce mistakes
* Always verify the participation thread before awarding
* Use the optional note field to share agendas or meeting links
* Award participation _after_ discussion ends, not during
