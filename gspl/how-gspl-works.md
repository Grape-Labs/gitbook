---
description: Learn how GSPL works on Governance
---

# How GSPL Works

The Program is located at address GovyJPza6EV6srUcmwA1vS3EmWGdLSkkDafRE54X1Dir and manages two account types: GovBoardingConfig and GovernanceEntry.

## GovBoardingConfig

GovBoardingConfig has three executable instructions associated with it that serve the following purposes:

- createConfig: This will create the initial configuration of the root directory.  Three key accounts associated with this instruction that need further clarification are:
  - config: The public key address of the configuration created.  This address identifies the unique root directory for which all governance entries will be associated with. 
  - admin: The public key address of the admininstrator who will have the rights to approve and deny requests associated with the specific GSPL configuration create.
  - verifierRealm: The public key address of the DAO (typically the calculated PDA address identified by the realms unique name in Governance UI) that we wish to associate as the parent realm of our configuration/root directory. 

## GovernanceEntry
