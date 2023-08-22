# Hydra-S2

The [Hydra-S2 ZK proving scheme](https://github.com/sismo-core/hydra-s2-zkps) is the second proving scheme in the Hydra family:

* Hydra = using Hydra Delegated Proof of Ownership via the [Commitment Mapper](../commitment-mapper.md)
* S2 = single source, version 2: optional verifications and [Vault Identifier](../vault-and-proof-identifiers.md)

The proving scheme expands on the [Hydra-S1 proving scheme](hydra-s1.md) and introduces the notion of a Vault Identifier while offering a modular way of creating ZK proofs.

### Vault Identifiers

[VaultId](../vault-and-proof-identifiers.md) is a new notion introduced with the Hydra-S2 proving scheme. This VaultId is deterministically generated from a `vaultSecret` and an application identifier (`appId`) by taking the Poseidon hash of these two values.&#x20;

This differs from the Proof Identifier used in the Hydra-S1 proving scheme, which functions as a nullifier—preventing the same proof from being used more than once. Meanwhile, a Vault Identifier functions as an anonymous app-specific identifier that can be utilized as an in-app user ID.

### Hydra Proof Of Ownership

Hydra Proof of Ownership differs in the Hydra-S2 proving scheme as far as a new Commitment Mapper and new variable when creating commitments are being used. The secret used to generate the commitment for the trusted Commitment Mapper is now the hash of the Vault secret.

[Hydra Proof of Ownership](./) also changes in Hydra-S2 as far as a new [Commitment Mapper](../commitment-mapper.md) and a new variable when creating commitments are being used. The secret used to generate the commitment for the trusted [Commitment Mapper](../commitment-mapper.md) is now the hash of the Vault secret and the user secret instead of the user secret only.

$$
Commitment = PoseidonHash(VaultSecret, AccountSecret);
$$

To allow this, we created a new Commitment Mapper, and we ensured that a Data Source can only be added to a single Data Vault, as the commitment is now based on the Vault secret.

### Optional Verifications

Hydra-S2 also offers a modular way of generating ZK proofs. Therefore, while [Hydra-S1](hydra-s1.md) obliged users to prove that they own two accounts and that the source account was in a specific [Account Tree](../../data-groups/what-is-the-data-vault-1/accounts-registry-tree.md) (itself in a specific [Registry Tree](../../data-groups/what-is-the-data-vault-1/accounts-registry-tree.md)) with a specific value and checking a nullifier value, we can avoid checking specific constraints in the circuits.

During the Hydra-S2 proving scheme, it is now optional to prove the following:

* Any account ownership via Hydra Proof Of Ownership and the new commitment generation
* Any Registry Tree or Account Tree Merkle Proof of inclusion
* Any value held by a source of data (Ethereum address, web2 account, etc.)
* Any [Vault Identifier](../vault-and-proof-identifiers.md) generation
* Any [Proof Identifier](hydra-s1.md) generation
