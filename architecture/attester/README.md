# Attester

An **Attester** is a smart contract generating and recording attestations in the Sismo Attestations State.&#x20;

It triggers the creation or update of an attestation after verifying that a claim made by a user is valid. A user can prove its claim is valid by generating a proof using the Proving Scheme associated to the Attester. The Attester verifies this proof against the Claims Datastore it reads from and according to the set of attester rules configured.

![](<../../.gitbook/assets/Sismo Attester.png>)

Once a claim is verified, the Attester records the attestation it generates in the correct attestation collection associated with the claim.

Each type of claim supported by an authorized Attester is associated to an attestation collection slot in the attester's reserved shard of the SAS.

An Attester can be configured with rules such as preventing a source to be used multiple times per attestation (by maintaining a Nullifier merkle tree), enabling revokation of attestations (by maintaining a Revoke merkle tree), or any other rule that can be coded into its logic.

One instance of attester needs to be deployed per Attestations Registry host (blockchain or hosted database.

Each Attester is linked to one:&#x20;

* **Claims Datastore**
  * A database which contains supported claims data _(e.g. a merkle tree containing a sub-tree of accounts holding CryptoPunks)_
* **Proving Scheme** comprised of:
  * A prover: code that enables users generate proofs from the claims data and source accounts signets, _(e.g I own an address from the list of cryptopunk owners)_
  * A verifier: code that enables anyone to check this proof and validate the claim.

## Proving Schemes

A **Proving Scheme** enables users to generate proofs for a defined set of claims (such as "I claim that I own an account that holds a Cryptopunk") and for anyone to verify those proofs.

There are currently 2 types of Proving Schemes maintained by Sismo Genesis Team:

{% content-ref url="smps-simple-merkle-proving-schemes.md" %}
[smps-simple-merkle-proving-schemes.md](smps-simple-merkle-proving-schemes.md)
{% endcontent-ref %}

{% content-ref url="zk-smps-zero-knowledge-simple-merkle-proving-scheme.md" %}
[zk-smps-zero-knowledge-simple-merkle-proving-scheme.md](zk-smps-zero-knowledge-simple-merkle-proving-scheme.md)
{% endcontent-ref %}

SMPS is based on public merkle proofs while ZK-SMPS generate and verify merkle proofs in the form of SNARKs, guaranteeing privacy for the user making the claim.

## Claims Datastore

A **Claims Datastore** is&#x20;



