---
description: A single source proving scheme that preserves the user's privacy
---

# ZK-SMPS (Zero Knowledge Simple Merkle Proving Scheme) Attester

ZK-SMPS Attester is an Attester using the Zero-Knowledge Simple Merkle Proving Scheme ([ZK-SMPS](https://github.com/sismo-core/ZK-SMPS)), a Zero Knowledge-based proving scheme developed by Sismo.&#x20;

This attester enables users to generate attestations in a privacy-preserving manner, meaning there is no link made between the source accounts used to make the claims and the destination that will own the attestation.

It checks claims coming from a single source account (no aggregation of source accounts data) and only allows for one single attestation per attestation collection to be generated from a specific source account.

## Attestation Rules

* Each attestationCollection of the attester corresponds to a claim (example of claim: punk owner, bayc owner, etc.)
* Nullifier
* Prove ownership destination

## Proving Scheme

## Claims Datastore

## Commitment Mapper

