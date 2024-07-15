---
description: Learn how GSPL works on Governance
---

# How GSPL Works

The Program is located at address GovyJPza6EV6srUcmwA1vS3EmWGdLSkkDafRE54X1Dir and manages two account types: GovBoardingConfig and GovernanceEntry.

## GovBoardingConfig

GovBoardingConfig has three executable instructions associated with it that serve the following purposes:

- createConfig: This will create the initial configuration of the root directory.  Executing this instruction we can assign the public key address of the admin.  The public key address of the config and the associated verifierRealm address (a calculated PDA address identifieb by the realms unique name in Governance UI) we wish to associated our configuration with. 

## GovernanceEntry
