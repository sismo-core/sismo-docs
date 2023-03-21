# Hydra-S2

The [Hydra-S2 ZK Proving Scheme](https://github.com/sismo-core/hydra-s2-zkps) is the second proving scheme of the Hydra Family:

* Hydra = using Hydra Delegated Proof of ownership ([via commitment mapper](../commitment-mapper.md))
* S2 = Single Source, Version 2: optional verifications and Vault Identifier

It takes its inspiration from the work done on [Hydra-S1](hydra-s1.md) and introduces the notion of a Vault Identifier while offering a modular way of creating ZK Proofs.

### Vault Identifier & Proof Identifiers

The [Vault identifier](../vault-and-proof-identifiers.md) (vaultId) is a new notion introduced with Hydra-S2, it is an anonymous app-specific identifier that can be utilized as an in-app user ID.

This vaultId is deterministically generated from a `vaultSecret` and an application Identifier (`appId`) by taking the Poseidon hash of these two values.&#x20;

A [Proof Identifier](../vault-and-proof-identifiers.md) can be seen as the nullifier used in Hydra-S1, as far as it is deterministically generated from the poseidon hash of a hashed sourceSecret and a requestIdentifier. The request Identifier is made of an application id, a group id, a group timestamp and a namespace (you can learn more about it in the [zkConnect package documentations](../../technical-documentation/zkconnect/)).

{% hint style="info" %}
Vault and Proof Identifiers are two tools with different purposes. A Vault identifier can help you to privately keep track of a user with an anonymous app-specific ID while a Proof Identifier can help you to prevent a user from using the same proof two times in your app.
{% endhint %}

### [Hydra Proof Of Ownership](./)

The Hydra Proof Of Ownership also changes in Hydra-S2 as far as we now use a new commitment mapper and a new variable when creating commitments. The secret used to generate the commitment to the trusted [Commitment Mapper](../commitment-mapper.md) is now the hash of the vault secret and the user secret instead of the user secret only.

$$
Commitment = PoseidonHash(VaultSecret, AccountSecret);
$$

To allow this, we created a new commitment mapper and we ensure that an account can only be added to one vault since the commitment is now based on the vault secret.&#x20;

### Optional verifications

Hydra-S2 also offers a modular way of generating ZK Proofs. Therefore, while [Hydra-S1](hydra-s1.md) obliged users to prove that they own two accounts and that the source account was in a specific [Account Tree](../accounts-registry-tree.md) (itself in a specific [Registry Tree](../accounts-registry-tree.md)) with a specific value and checking a nullifier value, we can avoid checking specific constraints in the circuits.&#x20;

It can be now optional to prove while using Hydra-S2 Proving Scheme:

* any account ownership via Hydra Proof Of Ownership and the new commitment generation
* any Registry Tree or Account Tree Merkle Proof of inclusion
* any value hold by a source of data (ethereum address, web2 account...)
* any Vault Identifier generation
* any Proof Identifier generation
