# T15\_ Grape.sol Subdomains

GRAPE Rewards & Emissions Management System (GREMS)\
Version 1.0 | Last Updated: 2025-12-30

Rewarding contributions, participation, and impact across the GRAPE DAO

## 1. Purpose

The **Grape.sol Subdomain** tab is a simple registry of GRAPE DAO community members who claimed a **grape.sol** subdomain. It ties each subdomain request to a **Discord name** and **wallet address** so the DAO has a clear reference for:

* Confirming which wallet a subdomain was associated with at the time of request
* Reducing duplicate or conflicting subdomain claims
* Providing a historical audit trail for future DAO programs involving names/subdomains

{% hint style="info" %}
Note: This tab is an informational registry. It does not calculate rewards and does not feed into emissions or recurring payments.
{% endhint %}

## 2. Tab Structure

The tab contains three columns:

|            |              |                                                                |
| ---------- | ------------ | -------------------------------------------------------------- |
| **Column** | **Field**    | **Meaning**                                                    |
| A          | Discord Name | The member’s Discord handle as recorded at the time of request |
| B          | Wallets      | The wallet address provided by the member                      |
| C          | Subdomains   | The requested **grape.sol** subdomain (if assigned/recorded)   |

Figure 1. _Grape.sol Subdomain Tab — Discord Name, Wallet, and Subdomain Registry_

## 3. Data Entry Method

Entries are recorded **manually** based on member submissions in the dedicated Discord channel (example: **#grape.sol-subdomain**). Typical workflow:

{% stepper %}
{% step %}
### Member posts request

Member posts their desired subdomain name + wallet address in the channel.
{% endstep %}

{% step %}
### Operator records entry

An operator copies the Discord name, wallet, and requested subdomain into this tab.
{% endstep %}

{% step %}
### Tab as canonical log

The tab serves as the canonical “who requested what” log for that campaign window.
{% endstep %}
{% endstepper %}

## 4. Usage and Controls

### 4.1 Primary Uses

* **Ownership reference:** quickly check which wallet was linked to a given subdomain request
* **Duplication control:** detect repeated subdomain requests or conflicting names
* **DAO memory:** retain a long-term record of subdomain assignments

### 4.2 Controls / Best Practices

* Treat the wallet address as the primary identifier for resolving duplicates (Discord names can change).
* Keep entries consistent (one row per member/wallet unless the DAO explicitly supports multiple subdomains per wallet).
* If a request is edited or corrected, append a note or use a clear update convention so the history remains auditable.
