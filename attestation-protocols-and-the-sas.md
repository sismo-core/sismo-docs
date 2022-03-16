---
cover: .gitbook/assets/TWITTER BANNERvert_1500x500px.jpeg
coverY: 0
---

# Sismo Attestations State

Sismo protocol will be the the host of several attestation protocols, authorized by Sismo DAO to write in the Sismo Attestations State (SAS).

The SAS is the cross-chain aggregated databases of all attestations issued by the Sismo protocol. Only the attesters of authorized attestation protocols are allowed to mutate the SAS.

Each authorized attestation protocol gets a (multi-chain) slot in the SAS.

\[Scheme attestations protocol <> SAS]



## Attestation Protocols

An attestation protocol is a protocol that enables users to generate attestations from their web3 source accounts.

We will take two attestation protocols as examples to define what an attestation protocol is:&#x20;

* ZK-SAP (for Zero Knowledge Single-source Attestation Protocol): our first Zero Knowledge Attestation protocol, that allows users to generate attestation from source accounts without revealing them
* S-SAP (for Simple Single-source Attestation Protocol)

### Attestation Proving Scheme

Each attestation protocol has an underlying attestation proving scheme:

* one prover: allow users to generate from their source accounts the attestation proofs.
* one verifier: allow anyone to verify the validity of an attestation request. It takes an an input the attestion proof.
* sourceOfTruth: Used to generate the proofs, and verify them.

For instance in ZK-SAP a ZK attestation protocol

* prover: ZK-SNARK prover that allows anyone to generate a ZK Proof that they own an address that is part of a merkle tree of addresses eligible for an attestation.
* verifier: is able to check whether a ZK Proof is valid or not
* sourceOfTruth: Database of all merkle trees of addresses eligible for an attestation.

In a more simple

* prover:&#x20;



