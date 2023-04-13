# Hydra-S2

The [Hydra-S2 ZK proving scheme](https://github.com/sismo-core/hydra-s2-zkps) is the second proving scheme in the Hydra family:

* Hydra = using Hydra Delegated Proof of Ownership via the [Commitment Mapper](../commitment-mapper.md)
* S2 = single source, version 2: optional verifications and [Vault Identifier](../vault-and-proof-identifiers.md)

The proving scheme expands on the [Hydra-S1 proving scheme](hydra-s1.md) and introduces the notion of a Vault Identifier while offering a modular way of creating ZK proofs.

### VaultId & Proof Identifiers

[VaultId](../vault-and-proof-identifiers.md) is a new notion introduced with Hydra-S2; it is an anonymous app-specific identifier that can be utilized as an in-app user ID.

This VaultId is deterministically generated from a `vaultSecret` and an application Identifier (`appId`) by taking the Poseidon hash of these two values.&#x20;

A [Proof Identifier](../vault-and-proof-identifiers.md) can be seen as the nullifier used in Hydra-S1, as far as it is deterministically generated from the Poseidon hash of a hashed sourceSecret and a requestIdentifier. The request Identifier is made of an application id, a group ID, a group timestamp, and a namespace (learn more about it in the [Sismo Connect package documentation](../../technical-documentation/sismo-connect/)).

{% hint style="info" %}
VaultId and Proof Identifiers are two tools with different purposes. VaultId can help you to privately keep track of a user with an anonymous app-specific ID while a Proof Identifier can help you to prevent a user from using the same proof two times in your app.
{% endhint %}

### Hydra Proof Of Ownership

[Hydra Proof of Ownership](./) also changes in Hydra-S2 as far as a new [Commitment Mapper](../commitment-mapper.md) and a new variable when creating commitments. The secret used to generate the commitment for the trusted [Commitment Mapper](../commitment-mapper.md) is now the hash of the Vault secret and the user secret instead of the user secret only.

$$
Commitment = PoseidonHash(VaultSecret, AccountSecret);
$$

To allow this, we created a new commitment mapper, and we ensure that an account can only be added to one vault since the commitment is now based on the vault secret.&#x20;

### Optional verifications

Hydra-S2 also offers a modular way of generating ZK proofs. Therefore, while [Hydra-S1](hydra-s1.md) obliged users to prove that they own two accounts and that the source account was in a specific [Account Tree](../accounts-registry-tree.md) (itself in a specific [Registry Tree](../accounts-registry-tree.md)) with a specific value and checking a nullifier value, we can avoid checking specific constraints in the circuits.&#x20;

It can be now optional to prove while using Hydra-S2 Proving Scheme:

* any account ownership via Hydra Proof Of Ownership and the new commitment generation
* any Registry Tree or Account Tree Merkle Proof of inclusion
* any value held by a source of data (Ethereum address, web2 account, etc.)
* any Vault Identifier generation
* any Proof Identifier generation
