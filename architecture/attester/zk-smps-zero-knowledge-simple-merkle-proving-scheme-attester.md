---
description: A single source proving scheme that preserves the user's privacy
---

# ZK-SMPS (Zero Knowledge Simple Merkle Proving Scheme) Attester

ZK-SMPS Attester is an Attester using the Zero-Knowledge Simple Merkle Proving Scheme ([ZK-SMPS](https://github.com/sismo-core/ZK-SMPS)), a Zero Knowledge-based proving scheme developed by Sismo.&#x20;

This attester enables users to generate privacy-preserving attestations creating no link at all between the source accounts used to make the claims and the destination that will own the attestation.

It checks claims coming from a single source account (no aggregation of source accounts data) and only allows for one single attestation per attestation collection to be generated from a specific source account.

![](<../../.gitbook/assets/Sismo Attester ZK-SMPS.png>)

## Attestation Rules

* **Source and destination account ownership requirement**
  * The user has to prove ownership of the source and destination account of the attestation.
* **1 claim per attestation collection**
  * Each type of claim _(e.g.: "I own a specific NFT"_) made through this attester can only be linked to one single attestationCollection.
* **Duplicate attestation prevention**
  * The attester stores external nullifiers in an invalidation merkle tree to prevent users from generating the same attestation more than once from the same source accounts.&#x20;
* **Revocation feature**
  * The attester stores attestation revocation in a revocation merkle tree to allow users to revoke an attestation and bypass the "duplicate attestation prevention" mechanism and to enable them to generate a new attestation if required.

## Proving Scheme



## Claims Datastore

## Commitment Mapper

With current technological limitations, verifying ECDSA signatures of Ethereum addresses inside a zk-SNARK circuit takes a significant time ([circom-ecdsa](https://github.com/0xPARC/circom-ecdsa/blob/master/README.md) 1.5M constraint and 1Go proving key). To solve this issue, Sismo uses the EdDSA digital signature scheme verification instead, which reduces considerably the verification time (5k constraint) to a value more compatible with standard user experience accessibility levels.

A module called the Commitment Mapper acts a a trusted third party to map a user's source Ethereum account to an EdDSA public key, allowing for faster verification in the zk-SNARK circuit.

In practice, a user would first prove ownership of an Ethereum account to the Commitment mapper, generate a secret, hash it and share it with the Commitment Mapper. The Commitment Mapper would store that proof of ownerhip^sxcwcwds\


