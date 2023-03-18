# Hydra-S1

The [Hydra-S1 ZK Proving Scheme](https://github.com/sismo-core/hydra-s1-zkps) is the first proving scheme of the Hydra Family:

* Hydra = using Hydra Delegated Proof of ownership ([via commitment mapper](../commitment-mapper.md))
* S1 = Single Source, Version 1: one group membership verification

It enables users to prove, **in one ZK Proof,** that for a given external nullifier and for a defined [Registry Merkle Tree filled with Accounts Trees](../accounts-registry-tree.md):&#x20;

* They own 2 accounts
  * A source account ([Hydra Proof Of Ownership](./#hydra-proof-of-ownership))
  * A destination account ([Hydra Proof Of Ownership](./#hydra-proof-of-ownership))
* The source account is part of an accounts tree ([Accounts Tree Merkle Proof](../accounts-registry-tree.md))
* This accounts tree was registered in a registry tree with a specific value ([Registry Tree Merkle Proof](../accounts-registry-tree.md))
* A claim about their source account value is true:&#x20;
  * e.g: "my account value is superior to 5" (non strict claim)&#x20;
  * or "my account value is strictly equal to 5" (strict claim)
* They correctly generated a nullifier by hashing the externalNullifier with the secret from the source account (a.k.a IdNullifier)

The nullifier can be stored by the verifier to make sure that a user cannot use two ZKPs for the same external nullifier.

The Hydra-S1 ZK Proving Scheme is used by the [Hydra S1 Attesters](https://github.com/sismo-core/sismo-protocol/tree/main/contracts/attesters/hydra-s1) of the Sismo Protocol.

{% hint style="info" %}
In the Hydra S1 Simple Attester, we use the Hydra S1 proving scheme to let a user:

* Prove they own a source account that's part of a specific Group of accounts identified by a group identifier. (the accounts tree value in the registry tree = groupIdentifier)
* Prove they own a destination account (that will receive the destination)
* Make a claim about the value of the account inside the Group.&#x20;
* generate a nullifier that will be saved on-chain inside the attester.&#x20;

The externalNullifier is defined as being the group identifier. This makes sure a user cannot generate two attestations per groups.
{% endhint %}

All these steps are executed inside the hydra-s1 circom circuit which is available [here](https://github.com/sismo-core/hydra-s1-zkps).
