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
- verifier: The new verifier public address of the verifier that will be associated with the specific GovBoardingConfig.
- verifierType: An enum-like field indicating the current verifier type such as:
  - 0 -> "VerifierRealm": A verifier that is tied to a specific DAO or governance realm.
  - 1 -> "VerifierOverride":  A more flexible verifier type that might have broader or overridable permissions.
    
### Additional considerations for GovBoardingConfig

  - Security: The admin public key should be carefully managed to prevent unauthorized access and modifications to the DAO's governance structure.
  - Flexibility: The verifierRealm concept allows for nested governance structures, enabling hierarchical or federated DAO models.
  - Governance Rules: While not explicitly mentioned in the GovBoardingConfig, the creation of this configuration often involves defining governance rules, voting mechanisms, and other parameters that dictate how the DAO operates.

## GovernanceEntry

The GovernanceEntry struct type defines the format for listing a DAO and each entry is essentially a record withing a specific GovBoardingConfig directory.  In consists of the following key fields:

- config: A public key reference to the parent GovBoardingConfig account. This field establishes the hierarchical relationship between the GovernanceEntry and its governing configuration.
- governanceProgram: A public key identifying the program responsible for handling governance-related logic and interactions for this GovernanceEntry.
- govAccountType: A string specifying the type or category of the GovernanceEntry, providing context for its purpose within the DAO.  The fields value is of type "DAOType" and can currently hold one of two values, "Social" or "Finance".
- requestStatus: An enum-like field indicating the current status of the GovernanceEntry, such as:
  - 0 -> "Pending"
  - 1 -> "Approved"
  - 2 -> "Rejected"
  - 3 -> "Disabled"
- rank: A numerical value used to order or prioritize GovernanceEntries within a specific context.
- parentLinks: A complex data structure (likely an array) defining relationships between GovernanceEntries, forming a hierarchical / graph-like structure.
- childRealms: A list of public keys representing child GovernanceEntries or sub-DAOs associated with this GovernanceEntry.
- name: A simple text name or identifier for the GovernanceEntry.
- metadataUri: A URI pointing to additional metadata or information related to the GovernanceEntry, stored off-chain.

### createEntry

A GovernanceEntry is initialized through the createEntry instruction. This instruction initiates a governance entry listing request within a specific DAO directory.  This instruction requires the following key accounts:

- payer: The public key address of the account initiating the request listing who also serves as the transaction signer and fee payer.
- config: The public key address of the existing GovBoardingConfig directory where the listing will be associated with.
- governanceEntry: A structured data type containing detailed information about the listing request. Details on the structure type are mentioned above.
  - Key things to take into consideration with the values in the governanceEntry struct:
    - requestStatus: The initial requestStatus should be set to 0 to indicate a "Pending" state.
    - name: The name field must be unique within the specified DAO directory and should match the exact name used in the DAO's governance program.
- governanceProgram: The public key address of the governace program of which the DAO belongs to (i.e. SPL Governance address GovER5Lthms3bLBqWub97yVrMmEogzX7xNjdXpPPCVZw)

By providing this information, the createEntry instruction initiates the process of adding a new listing to the DAO's governance structure, subject to approval by the DAO's administrators.

### setEntryStatus

### setEntryUri

### setChildStatus


