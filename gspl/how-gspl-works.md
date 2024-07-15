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
- verifierOverride: The account signer and fee payer for the GovBoardingConfig creation.  This account serves as the gatekeeper, ensuring that changes to the configuration are authorized and aligned with the GovBoarding structure.  It provides an additional layer of security and control over the fundamental settings.

### updateAdmin

The updateAdmin instruction is responsible for modifying the admin associated with a given GovBoardingConfig.  This allows an exisitng GovBoardingConifg to change the authority that approves or denies listing-related actions within that specific configuration.  In order to update the admin, authorization is required from the current verifier to prevent unauthorized changes. Listed below are the key accounts for updateAdmin:

- config: The public key address that identifies the existing created GovBoardingConfig root entry.
- admin: The public key address of the existing administrator.
- newAdmin: The publick key address of the new adminstrator tha the specific config needs to be changed to.
- verifier: The account signer and fee payer that typically is also the GovBoardingConfig creator.

### updateVerifier

The updateVerifier instruction is designed to modify the Verifier account associated with a GovBoardingConfig. This allows the DAO to change the entity responsible for authorizing modifications to the configuration.  In this special case the admin is the signer that has the authority to change the verifier.  Listed below are the key accounts for updateVerifier:

- admin: The public key address of the existing administrator and in this case the signer and fee payer of the transaction.
- config: The public key address that identifies the existing created GovbBoardingConfig root entry.
- verifier: The new verifier public address of the verifier that will be associated with the specific GobBoardingConfig.
- verifierType: ?



### Additional considerations for GovBoardingConfig
  - Security: The admin public key should be carefully managed to prevent unauthorized access and modifications to the DAO's governance structure.
  - Flexibility: The verifierRealm concept allows for nested governance structures, enabling hierarchical or federated DAO models.
  - Governance Rules: While not explicitly mentioned in the GovBoardingConfig, the creation of this configuration often involves defining governance rules, voting mechanisms, and other parameters that dictate how the DAO operates.

## GovernanceEntry
