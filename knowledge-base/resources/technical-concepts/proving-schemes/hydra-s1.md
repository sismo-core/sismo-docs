# Hydra-S1

The Hydra-S1 proving scheme is the first proving scheme in the Hydra family:

* Hydra = using Hydra Proof of Ownership via the [Commitment Mapper](../commitment-mapper.md)
* S1 = single source, version 1: verifying ownership of a single Data Gem

Currently, the proving scheme is utilized by the ZK Badge protocol **that is not maintained anymore.** It allows users to verify ownership of a Data Gem on a single Data Source and mint a ZK Badge on an unlinked Ethereum address.

{% hint style="info" %}
Sismo’s [ZK Badge protocol](https://github.com/sismo-core/sismo-badges) uses Hydra S1 Attesters to issue ZK Badges. You can see Sismo’s Hydra S1 attesters on GitHub [here](https://github.com/sismo-core/sismo-protocol/tree/main/contracts/attesters/hydra-s1). While the contracts are deployed, we stopped maintaining ZK Badge protocol.
{% endhint %}

## Hydra Proof of Ownership

The Hydra-S1 proving scheme allows participants to establish, in one ZK proof, that for a given Proof Identifier:

* They own 2 accounts (source and destination)
* The source account is part of an [Accounts Registry Tree](../accounts-registry-tree.md)
* The source account was registered in the [Accounts Registry Tree](../accounts-registry-tree.md) with a specific value
* A claim about their source account value is true
  * e.g: "my account value is superior to 5" (non-strict claim)
  * or "my account value is strictly equal to 5" (strict claim)
* They correctly generated a [Proof Identifier](../../../../build-with-sismo-connect/technical-documentation/vault-and-proof-identifiers.md)

{% hint style="info" %}
Proof Identifiers can be stored by verifiers to ensure a user cannot use two ZKPs for the same function when unintended.
{% endhint %}
