---
description: A private, local and sovereign identity aggregator.
---

# Overview

The Data Vault is a private, local and sovereign identity aggregator for Data Sources—imported externally-owned accounts (EOAs) or native [Vault Identifiers](vault-and-proof-identifiers.md) (vaultId).

It functions as the prover in proving schemes, enabling the generation of zero-knowledge proofs (ZKPs) that attest ownership of Data Sources.

Currently, the Data Vault supports the following types of Data Sources:

* Web3 accounts (imported Ethereum addresses, or EOAs, owned by a private key)
* Web2 accounts (imported GitHub, Twitter and Telegram)
* Vault Identifiers (native anonymous app-specific identifiers used to verify unique users)

{% hint style="warning" %}
A Data Source can only be imported into a single Data Vault. As a result, applications can verify users are unique without infringing on their privacy.
{% endhint %}

After aggregating Data Sources in the Data Vault, users can selectively disclose personal data to applications with Sismo Connect. Linking accounts has no implications on user privacy due to the encrypted nature of the Vault.

## Proving Ownership of Data Sources

Technically speaking, the Data Vault stores the following:

* The Vault Secret, obtained when creating a Data Vault
* Commitment receipts, obtained when importing Data Sources into the Data Vault

When creating a Data Vault, users generate a Vault Secret. The Vault Secret is unique and used to prove ownership of Data Sources in the Data Vault.

### Imported Data Sources

Users can import Ethereum addresses (EOAs) and web2 accounts such as GitHub, Twitter and Telegram into the Data Vault.

Importing Data Sources into the Vault creates a commitment—a hash of the Vault secret and an account identifier used in proving schemes.

A commitment is computed using the following formula:

`Commitment = Hash(VaultSecret, Account)`

This commitment is submitted to Sismo’s Commitment Mapper, an offchain trusted service that privately associates a commitment with a Data Source. Web3 accounts are associated with the Commitment Mapper by signing wallet messages, while web2 accounts are mapped via an OAuth authorization process.

After submitting a commitment to the Commitment Mapper, the Data Vault receives a commitment receipt. This receipt is used to verify proof of ownership in a zk-SNARK during proving schemes.

{% hint style="info" %}
Read more about the Commitment Mapper [here](commitment-mapper.md).
{% endhint %}

## Proving Ownership of Data Sources

Technically speaking, the Data Vault stores the following:

* The Vault Secret, obtained when creating a Data Vault
* Commitment receipts, obtained when importing Data Sources into the Data Vault

When creating a Data Vault, users generate a Vault Secret. The Vault Secret is unique and used to prove ownership of Data Sources in the Data Vault.

### Native Data Sources

In addition to imported Data Sources, the Data Vault has native Data Sources in the form of Vault Identifiers. A Vault Identifier (`vaultId`) functions an as anonymous app-specific user ID. It is computed using the following formula:

`vaultId = Hash(VaultSecret, appId)`

Sismo Connect applications leverage Vault Identifiers to verify that users are unique, thus preventing double spends or other unintended actions. For example, Sismo Connect applications like SafeAirdrop and Privacy Is Normal have utilized vaultId to ensure user uniqueness while preserving user privacy. As a Data Source can only be imported into a single Data Vault, users always have the same vaultId, enabling applications to confirm that a user is unique and add a Sybil-resistance.

{% hint style="info" %}
Read more about Vault Identifiers [here](vault-and-proof-identifiers.md).
{% endhint %}

Vault Identifiers can be used as Data Sources themselves. During the Privacy Is Normal lottery, participants proved they were Tornado Cash users and Gitcoin Passport holders in a privacy-preserving manner. After doing so, the lottery winners were selected with Vault Identifiers, which were transformed into a Data Group—allowing winners to claim their prize.

{% hint style="success" %}
See the full Privacy Is Normal case study [here](https://case-studies.sismo.io/db/anon-lottery).
{% endhint %}

## Encrypted Storage

The Data Vault is encrypted and only ever exists in its decrypted state on a user’s device. As such, aggregating Data Sources in the Data Vault has no implications on user privacy.

The Vault decryption key, obtained when creating a Data Vault, is derived deterministically from the user’s ECDSA wallet signature and used to access the Data Vault. As the Data Vault seed is deterministic, it ensures a user can always decrypt their Data Vault with the same ECDSA wallet signature.

The wallet address used to create the Data Vault is its designated owner and first Data Source. Imported web3 accounts are designated as Data Vault owners by default, though this can be modified in the application’s settings. In addition, users can generate a recovery key to ensure they do not lose access to their Data Vault.
