---
description: App-specific anonymous user ID
---

# Vault Identifiers

Sismo uses zero-knowledge proofs (ZKPs) to let users prove ownership of anonymized granular pieces of personal data known as Data Gems. These Data Gems can be revealed to applications integrated with Sismo Connect without revealing the associated Data Source. For example, users can prove they own a certain NFT (i.e. Data Gem) without revealing the wallet address (i.e. Data Source) holding it.

Despite offering privacy, many applications still need to keep track of individual users to avoid double spends or enable more complex user management. Identifiers also need to remain specific to each application to avoid the possibility of tracking users across multiple applications.

## Sovereign & Anonymous ID for Sismo Connect Apps

A Vault Identifier, also known as `vaultId`, is calculated using the following formula:

`vaultId` = hash(`vaultSecret`, hash(`appId`, `derivationKey`))

Here's what each component represents:

1. **`vaultSecret`**: This is a confidential piece of information exclusive to a Data Vault’s owner. it must be understood as the seed/ private key of the user.
2. **`appId`**: This represents the unique identifier for the associated Sismo Connect application.
3. **`derivationKey`**: By default, this is set to zero. However, it can optionally be used by application developers to generate multiple user identifiers for a single Data Vault owner.

### Anonymous

Only the owner of a Data Vault knows their associated `vaultSecret`. As a result, they are the only person capable of computing their unique `vaultId` for a specific application. Computation occurs when users prove ownership of Data Gems during the generation of ZKPs. When the application verifies the ZKP, it receives the Vault Identifier as an output, thus authenticating the Data Gem owner as a unique user.

### App-Specific

The use of an application's **`appId`** in the formula ensures the uniqueness of Vault Identifiers across different applications. This feature eliminates any risk of cross-application exposure or doxxing.

### Deterministic

As a Vault Identifier is deterministically derived from a `vaultSecret` and `appId` , a single user cannot have multiple identifiers for a specific application unless they create another Data Vault. However, Data Sources can only be added to a single Data Vault, thus ensuring a user cannot use twice the same Data Source to prove a Data Gem ownership without the application knowing it.

### Native Data Sources

In addition, Vault Identifiers can be used as native Data Sources in the Sismo ecosystem. This allows for the grouping of Vault Identifiers to create Data Gems, which can be used on additional Sismo Connect applications.

## Case Studies

The following case studies contain examples of `vaultId` being used in a real-world context.

### Vault Identifier as a Nullifier

SafeAirdrop, a Sybil-resistant airdrop leveraging privately aggregated data, uses `vaultId` to prevent individual users from receiving an airdrop more than once. As users always compute the same `vaultId` on a specific application, they cannot access the application’s gated feature after already claiming the airdrop.

Although the application can identify whether a user is unique or not, privacy is not compromised as a `vaultId` does not contain any sensitive information and cannot be linked to other applications or the user’s private vault. In this context, `vaultId` acts as the nullifier frequently used by zero-knowledge protocols to prevent double spends.

{% hint style="success" %}
Read the full case study [here](https://case-studies.sismo.io/db/safe-drop).
{% endhint %}

### Vault Identifier as a Sybil-Resistant User ID

Privacy Is Normal is an anonymous Sybil-resistant lottery for Tornado Cash users. The application uses Vault Identifiers to identify lottery participants are unique individuals and select winners. Lottery winners were then able to claim their prize by proving their own a winning `vaultId`.

Despite knowing whether a user has already entered the lottery or not, the application has no access to sensitive information that could identify the user in question. As a result, `vaultId` functions as a Sybil-resistant user ID that allows the application to track user activity without infringing on their privacy.

{% hint style="success" %}
Read the full case study [here](https://case-studies.sismo.io/db/anon-lottery).
{% endhint %}
