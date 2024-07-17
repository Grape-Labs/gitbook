---
description: Find out how to quickly fetch verified DAOs from the GSPL
---

# Getting Started & Fetching Data from the GSPL

## Setting up the SDK

```
npm install \  
@metaplex-foundation/umi \  
@metaplex-foundation/umi-bundle-defaults \  
@solana/web3.js \
gspl-directory
```

## Initializing the GSPL

```
import {createUmi} from "@metaplex-foundation/umi-bundle-defaults"; 
import {getRealms, RequestStatus} from "gspl-directory";
import {publicKey as UmiPK} from "@metaplex-foundation/umi"; 
```

You may need to cast **publicKey** from the metaplex sdk if you are using **publicKey** from **web3** to avoid conflicts, the above shows a simple casting to avoid conflicts

## Fetching the GSPL Directory

Connecting Umi to an RPC is as simple as creating an umi instance and passing through your rpc end point as the first argument. It is recommended to use at least a free RPC end point from one of the many Solana RPC providers and not use the public endpoint of `https://api.mainnet-beta.solana.com` due to it's restrictions and limitations.

Connecting Umi to devnet is as simple as swapping out the RPC end point for that of a Devnet endpoint.

To use the getRealms the config public key must be passed, this config identifies the directory configuration that was used, in essence there can be multiple configs setup indipendantly of each other, the universal config directory has been set to **GrVTaSRsanVMK7dP4YZnxTV6oWLcsFDV1w6MHGvWnWCS**

```
            const CONFIG = UmiPK("GrVTaSRsanVMK7dP4YZnxTV6oWLcsFDV1w6MHGvWnWCS");
            const umi = createUmi(RPC_ENDPOINT);
            const entries = await getRealms(umi, CONFIG, RequestStatus.Approved);
            console.log("Entries ",entries);
```

## Example Fetching Summary

<figure><img src="../.gitbook/assets/Screenshot 2024-06-18 at 9.27.32 AM.png" alt=""><figcaption><p>A summary from the on chain GSPL data fetched from the program at https://governance.so</p></figcaption></figure>

## Example Verified DAO

<figure><img src="../.gitbook/assets/Screenshot 2024-06-18 at 9.27.53 AM.png" alt=""><figcaption><p>A verification example found on the directory page of https://governance.so fetched from the GSPL</p></figcaption></figure>
