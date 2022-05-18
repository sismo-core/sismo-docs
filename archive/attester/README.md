---
description: Writing in the global record of all Sismo attestations
---

# Attester

An **Attester** is a smart contract generating and recording attestations in the Sismo Attestations State.&#x20;

It triggers the creation or update of an attestation after verifying that a claim made by a user is valid. A user can prove its claim is valid by generating a proof using the Proving Scheme associated to the Attester. The Attester verifies this proof against the Claims Datastore it reads from and according to the set of attester rules configured.

![](<../../.gitbook/assets/Sismo Attester.png>)

Once a claim is verified, the Attester records the attestation it generates in the correct attestation collection associated with the claim.

Each type of claim supported by an authorized Attester is associated to an attestation collection slot in the attester's reserved shard of the SAS.

An Attester can be configured with attestation rules such as preventing a source to be used multiple times per attestation (by maintaining a Nullifier merkle tree), enabling revokation of attestations (by maintaining a Revoke merkle tree), or any other rule that can be coded into its logic.

One instance of attester needs to be deployed per Attestations Registry host (blockchain or hosted database).

Each Attester is configured to implement the verification part of a specific Proving Scheme and to read data from a Claims Datastore.

Anyone can implement and configure its own Attester and get it authorized by Sismo Attestation Protocol.

### Proving Schemes

A **Proving Scheme** enables users to generate proofs for a defined set of claims (such as "I claim that I own an account that holds a Cryptopunk") and for anyone to verify those proofs.

It is comprised of:

* **A prover**
  * Code that enables users generate proofs from the claims data and source accounts signets _(e.g I own an address from the list of cryptopunk owners)._
* **A verifier**
  * Code that enables anyone to check this proof and validate the claim.

### Claims Datastore

A **Claims Datastore** is the source of truth of an Attester. It stores data about potential claims (linked to web3 accounts) to be validated in a format adapted to different proving schemes _(e.g. a merkle tree containing a sub-tree of accounts holding CryptoPunks)._

## Current "Attester Configurations" (placeholder)

An Attester configuration is a setup of an attester with its associated proving scheme and claims datastore.

There are currently 2 Attester configurations maintained by Sismo Genesis Team:

{% content-ref url="zk-smps-zero-knowledge-simple-merkle-proving-scheme-attester.md" %}
[zk-smps-zero-knowledge-simple-merkle-proving-scheme-attester.md](zk-smps-zero-knowledge-simple-merkle-proving-scheme-attester.md)
{% endcontent-ref %}

{% content-ref url="smps-simple-merkle-proving-scheme-attester.md" %}
[smps-simple-merkle-proving-scheme-attester.md](smps-simple-merkle-proving-scheme-attester.md)
{% endcontent-ref %}

SMPS Attester Config is based on public merkle proofs while ZK-SMPS Attester Config generates and verifies merkle proofs in the form of SNARKs, guaranteeing privacy for the user making the claim.

An Attester Configuration can set up a new set of attestation rules, a new proving scheme or a proprietary claims datastore or simply be a fork of an existing Attester Configuration changing one of these.
