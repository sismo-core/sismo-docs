---
description: Data Groups and how to create them.
---

# Overview

Data Groups are created via the [Factory UI](https://factory.sismo.io) or by creating a pull request on the [Sismo Hub](https://github.com/sismo-core/sismo-hub).

<figure><img src="../.gitbook/assets/Sismo Connect_ Under the Hood.png" alt=""><figcaption></figcaption></figure>

The Factory is an interface to easily create Data Groups on the Sismo Hub infrastructure.&#x20;

The Sismo Hub computes Merkle trees of groups, stores them in its database and publishes the Merkle tree roots onchain.&#x20;

Onchain roots are the source of truth for back ends and smart contracts to verify whether a ZK proof is valid.

{% hint style="info" %}
Everything is [open-source](https://github.com/sismo-core/sismo-hub). Anyone can compute any Data Group.
{% endhint %}

## Data Sources

Data Sources are the accounts that users aggregate in their Data Vault. Currently supported Data Sources:&#x20;

* Ethereum wallets/EVM accounts
* GitHub accounts
* Twitter accounts
* Telegram accounts

From imported Data Sources, Vault owners can generate ZK proofs of:

* Ownership of a specific Data Source (i.e, authentication)
* Inclusion of an owned Data Source in a Data Group (and a claim about its value in the group)

{% hint style="info" %}
Take a look at the [Sismo Connect Cheatsheet](../build-with-sismo-connect/sismo-connect-cheatsheet.md) to see all requests.
{% endhint %}

## Data Groups

Data Groups are sets of Data Sources in which each Data Source has an associated value.

<details>

<summary>Concrete examples of Data Groups and ZK proofs you can request from users</summary>

```json
{ // "Stand With Crypto" NFT Minters Data Group
  "0xa2bf1b0a7e079767b4701b5a1d9d5700eb42d1d1": "2", // minted 2 NFTs
  "0xd03ad690ed8065edfdc1e08197a3ebc71535a7ff": "4", // minted 24 NFTs
  "0x70ddb5abf21202602b57f4860ee1262a594a0086": "21",// minted 21 NFTs
  "0x0e440bd9798ad22cb8fd6f1a433f2f16e8786770": "3", 
  "0x1e8cbbbfb827785ecc23dd0426a8907c7cdcca3a": "3",
  "0x4101ec64896fa8afda5be145b6321275bb375fe0": "3",
  "0x600f9faa8a2d39a710b28e2d0ec8a5dacc12b00f": "11",
  "0xc643c9411a6b489e9833b16631140f42bbfcb6d1": "2",
  "0x750f565251228a561d8ce8cceb03731a7a2430f8": "2",
  "0x2245be89fc8fab94ed982e859aa3212a4e4eb7e5": "14",
  [...]
}
```

* All owners of these wallets can generate a ZK proof attesting that they are part of this group

<!---->

* The owner of `0x70ddb5abf21202602b57f4860ee1262a594a0086` can generate a ZK proof attesting that they are part of the group with a value > 10 (e.g, minted more than 10 NFTs)
* The owner of 0xa2bf1b0a7e079767b4701b5a1d9d5700eb42d1d1 can create a ZK proof attesting that they are part of the group with a value = 2 (e.g minted exactly 2 NFT)

```json
{ // Sismo Community Data Group, created by Sismo
  // It regroups all community members, organized in 3 levels
  // level 1 = supporter, level 2 = contributor, level 3 = builder
  "0x32108e5f09f0df35aefc2ef4c520bbd06a57dae5": "2", // level 2 
  "0x53deea1808b6d2b8681241e3857b6c6ed1e7e103": "1", // level 1
  "0x1c494f1919c1512ebe74a5dcc17dac9a64069023": "2", // level 2
  "dhadrien.eth": "3", // level 3
  "github:yum0e": "2",
  "github:leosayous21": "2",
  "telegram:sampolgar": "2",
  "telegram:zpedro": "2",
  "twitter:wojtekwtf": "3",
  "twitter:albiverse": "3",
  [...]
}
```

* All owners of these wallets can generate a ZK proof attesting that they are part of this group

<!---->

* The owner of `dhadrien.eth` can create a ZK proof attesting that they are part of the group with a value > 2 (e.g, community member with level > 2)
* The owner of  @wojtekwtf on Twitter can generate  a ZK proof attesting that they are part of the group with a value = 3 (e.g, community member with level 3)



</details>

<table><thead><tr><th width="223.33333333333331">Data Group Examples</th><th>Members (Data Sources)</th><th>Value for each Data Source</th></tr></thead><tbody><tr><td><a href="https://factory.sismo.io/groups-explorer?search=0xfae674b6cba3ff2f8ce2114defb200b1">"Stand With Crypto" NFT Minters</a></td><td>Wallets of minters </td><td>Number of NFT minted</td></tr><tr><td><a href="https://factory.sismo.io/groups-explorer?search=0x1cde61966decb8600dfd0749bd371f12">Gitcoin Passport Holders</a></td><td>Wallets of Gitcoin Passport holders</td><td>Sybil-resistant score</td></tr><tr><td><a href="https://factory.sismo.io/groups-explorer?search=0xda1c3726426d5639f4c6352c2c976b87">Sismo Hub Github Contributors </a></td><td>GitHub accounts of contributors to sismo-core/sismo-hub repo</td><td>Number of contributions</td></tr><tr><td><a href="https://factory.sismo.io/groups-explorer?search=0x85c7ee90829de70d0d51f52336ea4722">ENS DAO Voters</a></td><td>Wallets of voters in the ENS DAO</td><td>Number of votes</td></tr><tr><td><a href="https://factory.sismo.io/groups-explorer?search=0xd630aa769278cacde879c5c0fe5d203c">Sismo Community Members</a></td><td>Wallets, GitHub, Telegram and Twitter accounts of all people that helped Sismo</td><td>Level of their contributions (1, 2 or 3)</td></tr></tbody></table>

<table data-view="cards"><thead><tr><th></th><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><p>Factory Guide: </p><p>Create a Data Group in 5 Minutes</p></td><td></td><td></td><td><a href="tutorials/create-your-data-group.md">create-your-data-group.md</a></td></tr><tr><td>Sismo Hub Guide: <br>Create Data Groups Programmatically</td><td></td><td></td><td><a href="tutorials/create-your-group-generator.md">create-your-group-generator.md</a></td></tr><tr><td>Sismo Hub Guide: <br>Add a Data Provider to the Sismo Factory</td><td></td><td></td><td><a href="tutorials/create-your-data-provider.md">create-your-data-provider.md</a></td></tr></tbody></table>
