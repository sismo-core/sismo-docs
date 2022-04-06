# Attester

An **Attester** is a smart contract implementing the verifier of a [Claiming Scheme](claiming-protocol/). It generates attestations after verifying the claims and their proofs provided by the user.

// DIAGRAM //

Once a claim is verified, the attester records a corresponding attestation by writing in the correct attestation collection associated with the claim.

Each claim supported by an authorized attester gets attributed an attestation collection in the attester's reserved shard of the Sismo Attestation State (SAS).

Specific rules can be set up in the attester such as allowing for the renewal of attestation or not, or allowing a source to be used only once per attestation or not.

Each claiming protocol will require at least one attester per registry per host (where the attestation is recorded : blockchain or database).

For example, the first two claiming protocols maintained by Sismo (SMCP and ZK-SMCP) will be first deployed with one authorized attester per blockchain supported.
