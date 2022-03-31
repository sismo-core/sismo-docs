---
cover: ../.gitbook/assets/TWITTER BANNERvert_1500x500px.jpeg
coverY: 0
---

# Attestation Protocols

Sismo protocol should allow anyone to generate attestations, easily integrable in their web2 and web3 applications.

Sismo is the host of several attestation protocols, authorized by Sismo DAO to write in the Sismo Attestations State (SAS).&#x20;

Each authorized attestation protocol gets a slot in the SAS.

\[Scheme attestations protocol <> SAS]

\[SCHEME 2: Attestation protocol: series of supported claims, a prover to generate attestation proof => create attestation through attester]

###

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





Like all attestation protocols,&#x20;

* Allows anyone to prove that they own an address that is part of a list of addresses, without revealing which address.
* Several claims can be attested using ZK-SAP
  * Claim #133: Proof that you own an address that owns a BAYC
  * Claim #22: Proof that you own an address that made a transaction before 2020

As ZK-SAP is the first authorized attestation protocol in Sismo, it controls the shard #1 of the Sismo Attestations State (SAS)

* BAYC Owners will be able using ZK-SAP, to receive an attestation for the Claim #133
* The corresponding user attestations are stored in the attestation collection slot #133, shard 1# of the SAS. Inside this collection lies all attestations created from the claim #133 of the ZK-SAP attestation protocol.



