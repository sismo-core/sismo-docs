# Hydra-S1: ZK Merkle Attester

The[ Hydra-S1 Simple Attester](https://github.com/sismo-core/sismo-protocol/blob/main/contracts/attesters/hydra-s1/HydraS1SimpleAttester.sol), which uses the Hydra-S1 Proving scheme ([docs](../../../technical-concepts/hydra-zk-proving-schemes/hydra-s1.md), [circuits](https://github.com/sismo-core/hydra-s1-zkps)), is a Merkle ZK Attester that verifies ZK proofs of merkle proof verifications and issues ZK Attestations.

ZK Attestations do not reveal anything about the source account(s) that was used to prove group ownership.

In their frontend, users download the groups and generate a ZK Proof from it using the Hydra-S1 prover.&#x20;

Users prove, in a zk-SNARK, that they own an account that's part of a group stored in an accounts merkle tree (registered in a Registry tree) and a destination account that will receive the attestation (so they prove consent to the attestation).

![Hydra-S1: ZK Merkle Attester](<../../../.gitbook/assets/4 (4).png>)

The attester verifies from users' ZK Proofs: &#x20;

* Ownership(s): the user owns two accounts - a source account and a destination account (via Hydra Delegate Proof of Ownership)
* Account inclusion: their source account is part of a Group of Accounts (e.g ENS DAO Voters)
* Account value: their source account holds a specific value (e.g number of votes in the group of ENS DAO voters)
* Nullifier: they computed a nullifier from an externalNullifier. \
  \
  The nullifier is deterministically generated from their source account and the externalNullifier. \
  \
  It is stored by the attester to make sure that a user cannot generate the same attestation on two destinations.

Please refer to the following documentation for more info:

* [Hydra-S1 docs](../../../technical-concepts/hydra-zk-proving-schemes/hydra-s1.md)
* [contracts](https://github.com/sismo-core/sismo-protocol/blob/main/contracts/attesters/hydra-s1/HydraS1SimpleAttester.sol)
* [circuits](https://github.com/sismo-core/hydra-s1-zkps)
