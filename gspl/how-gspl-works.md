---
description: Learn how GSPL works on Governance
---

# How GSPL Works

The Program is located at address GovyJPza6EV6srUcmwA1vS3EmWGdLSkkDafRE54X1Dir and manages two account struct types: GovBoardingConfig and GovernanceEntry.

## GovBoardingConfig

By creating a GovBoardingConfig, a DAO lays the groundwork for its governance processes.  

### createConfig

GovBoardingConfig is initialized through the createConfig instruction. This instruction establishes a root directory for governance entries and defines key associated accounts:

- config: The public key representing the unique identifier of the created root directory, acting as a namespace for associated governance entries.
- admin: The public key of the administrator responsible for overseeing and approving governance-related actions, such as managing listing approvals, within this configuration.
- verifierRealm: The public key address that references the parent DAO, acting as the overarching governance structure for this configuration, potentially inheriting certain rules or permissions.

### updateAdmin

The updateAdmin instruction is responsible for modifying the admin associated with a given GovBoardingConfig.  This allows an exisitng GovBoardingConifg to change the authority that approves or denies listing-related actions within that specific configuration.  In order to update the admin the required accounts are the following:

- config: The public key address that identifies the existing created GovBoardingConfig root entry.
- admin: The public key address of the existing administrator.
- newAdmin: The publick key address of the new adminstrator tha the specific config needs to be changed to.

### updateVerifier

### Additional considerations for GovBoardingConfig
  - Security: The admin public key should be carefully managed to prevent unauthorized access and modifications to the DAO's governance structure.
  - Flexibility: The verifierRealm concept allows for nested governance structures, enabling hierarchical or federated DAO models.
  - Governance Rules: While not explicitly mentioned in the GovBoardingConfig, the creation of this configuration often involves defining governance rules, voting mechanisms, and other parameters that dictate how the DAO operates.

## GovernanceEntry
