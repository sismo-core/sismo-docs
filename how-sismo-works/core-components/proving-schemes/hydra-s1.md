# Hydra-S1

The Hydra-S1 proving scheme is the first proving scheme in the Hydra family:

* Hydra = using Hydra Proof of Ownership via the [Commitment Mapper](../../technical-concepts/commitment-mapper.md)
* S1 = single source, version 1: verifying membership in a single [Data Group](../../../data-groups/data-groups-and-creation/)

Currently, the proving scheme is utilized by the ZK Badge protocol, **which is no longer maintained.** It allows users to verify membership in a Data Group from a single Data Source and mint a ZK Badge on an unlinked Ethereum address.

{% hint style="info" %}
Sismo’s [ZK Badge protocol](https://github.com/sismo-core/sismo-badges) uses Hydra S1 proving schemes to issue ZK Badges. You can see them [here](https://github.com/sismo-core/sismo-protocol/tree/main/contracts/attesters/hydra-s1). While the contracts are still deployed, Sismo no longer maintains the ZK Badge protocol. Read more in this [blog post](https://sismo.mirror.xyz/MimvqFv45hohMwDBD9rGqY4XGZIHRR8On7nx6q9YFRc).
{% endhint %}

## Hydra Proof of Ownership

The Hydra-S1 proving scheme allows participants to establish, in one ZK proof, that for a given Proof Identifier:

* They own 2 accounts (source and destination)
* The source account is part of an [Accounts Registry Tree](../../technical-concepts/accounts-registry-tree.md)
* The source account was registered in the [Accounts Registry Tree](../../technical-concepts/accounts-registry-tree.md) with a specific value
* A claim about their source account value is true
  * e.g: "my account value is superior to 5" (non-strict claim)
  * or "my account value is strictly equal to 5" (strict claim)
* They correctly generated a Proof Identifier

{% hint style="info" %}
A Proof Identifier functions as a nullifier for the Hydra-S1 proving scheme, preventing the same user from minting a certain ZK Badge more than once.&#x20;
{% endhint %}
