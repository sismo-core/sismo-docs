# Attester

An **Attester** is a smart contract generating and recording attestations in the Sismo Attestations State.&#x20;

It triggers the creation or update of an attestation after verifying that a claim made by a user is valid. A user can prove its claim is valid by generating a proof using the Proving Scheme associated to the Attester. The Attester verifies this proof against the Claims Datastore it reads from and according to the set of attester rules configured.

// DIAGRAM //

Once a claim is verified, the Attester records the attestation it generates in the correct attestation collection associated with the claim.

Each claim supported by an authorized Attester gets attributed an attestation collection slot in the attester's reserved shard of the SAS.

An Attester can be configured with rules such as preventing a source to be used multiple times per attestation (by maintaining a Nullifier merkle tree), enabling revokation of attestations (by maintaining a Revoke merkle tree), or any other rule that can be coded into its logic.

~~Each claiming protocol will require at least one attester per registry per host (where the attestation is recorded : blockchain or database)~~. (reformulate)\
\
// INTEGRATE FORMER CLAIMING PROTOCOL INFORMATION HERE //

For example, the first two claiming protocols maintained by Sismo (SMCP and ZK-SMCP) will be first deployed with one authorized attester per blockchain supported.
