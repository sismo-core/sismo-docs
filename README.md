---
description: Sovereign Identity Aggregator and Cryptonative SSO
cover: .gitbook/assets/Gibook Banner1_1900x400.png
coverY: 0
layout:
  cover:
    visible: true
    size: full
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# What is Sismo?

Sismo leverages zero-knowledge proofs (ZKPs) and privacy-preserving technologies to **enable** **users to aggregate their identities and selectively disclose personal data** to applications.&#x20;

By using Sismo Connect, an easy-to-integrate SSO, applications **can now obtain personal data that was previously inaccessible, while respecting users' privacy.**

[Sismo Connect](./#sismo-connect-the-crypto-native-sso) aims to replace non-sovereign SSOs such as Google Connect and improve limited SSOs such as Wallet Connect.&#x20;

<figure><img src=".gitbook/assets/Introduction (1).png" alt=""><figcaption></figcaption></figure>

## Data Vault: Sovereign Identity Aggregator

Users aggregate their identity by adding Data Sources to their sovereign, local and private [Data Vault](how-sismo-works/core-components/what-is-the-data-vault.md).

The Data Vault currently supports the following types of Data Sources: Ethereum wallets, GitHub, Twitter or Telegram accounts. Users can generate ZK Proofs from their Data Sources, enabling them to reveal data to applications in a sovereign way.

<figure><img src=".gitbook/assets/image.png" alt=""><figcaption><p>What a Data Vault looks like</p></figcaption></figure>

{% hint style="success" %}
You can create your own Data Vault and start aggregating your identity [here](https://vault-beta.sismo.io/).
{% endhint %}

## Sismo Connect: The Cryptonative SSO

Sismo Connect is a cryptonative single sign-on method (SSO) for onchain and offchain apps. Sismo Connect makes it easy for developers to obtain users' personal data by requesting and verifying ZK proofs.

{% hint style="success" %}
Experience Sismo Connect on the [Demo App Store](https://demo.apps.sismo.io)!
{% endhint %}

<figure><img src=".gitbook/assets/Sismo Connect Flow (4).png" alt=""><figcaption></figcaption></figure>

Integration is simple with just a few lines of code: import the front-end package or React button to make Sismo Connect requests, and verify proofs in your backend/ smart contracts using Sismo’s Solidity or TypeScript package.

Applications can request many data at once. There exist two types request that can be made:

* authentication: ZK Proof of Data Source ownership. (Ethereum wallets, GitHub, Twitter or Telegram accounts)
* disclosure of anonymised personal data: ZK Proofs of Data Source inclusion in a specific Data Group (+ claim about its value in the group).&#x20;

{% hint style="info" %}
Data Groups are sets of Data Sources where each Data Source has an associated value.
{% endhint %}

<details>

<summary>Concrete examples Data Groups and ZK Proofs you can request from users</summary>

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

* All owners of these wallets can create a ZK Proof that they are part of this group

<!---->

* owner of `0x70ddb5abf21202602b57f4860ee1262a594a0086` can create a ZK Proof that they are part of the group with value > 10 (e.g minted more than 10 NFTs)
* owner of 0xa2bf1b0a7e079767b4701b5a1d9d5700eb42d1d1 can create a ZK Proof that they are part of the group with value = 21 (e.g minted exactly 2 NFT)



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

* All owners of these wallets can create a ZK Proof that they are part of this group

<!---->

* owner of `dhadrien.eth` can create a ZK Proof that they are part of the group with value > 2 (e.g community member with level > 2)
* owner of  @wojtekwtf on twitter can create  a ZK Proof that they are part of the group with value = 3 (e.g community member with level 3)



</details>

Examples of Data Groups:

| Data Group Examples                                                                                                   | Members (Data Sources)                                                         | Value for each Data Source               |
| --------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------ | ---------------------------------------- |
| ["Stand With Crypto" NFT Minters](https://factory.sismo.io/groups-explorer?search=0xfae674b6cba3ff2f8ce2114defb200b1) | Wallets of minters                                                             | number of NFT minted                     |
| [Gitcoin Passport Holders](https://factory.sismo.io/groups-explorer?search=0x1cde61966decb8600dfd0749bd371f12)        | Wallets of Gitcoin Passport holders                                            | sybil-resistant score                    |
| [Sismo Hub Github Contributors ](https://factory.sismo.io/groups-explorer?search=0xda1c3726426d5639f4c6352c2c976b87)  | Github accounts of contributors to sismo-core/sismo-hub repo                   | number of contributions                  |
| [ENS DAO Voters](https://factory.sismo.io/groups-explorer?search=0x85c7ee90829de70d0d51f52336ea4722)                  | Wallets of voters in ENS DAO                                                   | number of votes                          |
| [Sismo Community Members](https://factory.sismo.io/groups-explorer?search=0xd630aa769278cacde879c5c0fe5d203c)         | Wallets, GitHub, Telegram and twitter accounts of all people that helped Sismo | level of their contributions (1, 2 or 3) |

{% hint style="info" %}
Anyone can [create a new Data Group](data-groups/data-groups-and-how-to-create-them/).&#x20;
{% endhint %}

<table data-view="cards"><thead><tr><th data-card-target data-type="content-ref"></th><th data-hidden data-card-cover data-type="files"></th></tr></thead><tbody><tr><td><a href="broken-reference">Broken link</a></td><td><a href=".gitbook/assets/Build with Sismo Connect.png">Build with Sismo Connect.png</a></td></tr><tr><td><a href="https://apps.sismo.io">https://apps.sismo.io</a></td><td><a href=".gitbook/assets/AppStore.png">AppStore.png</a></td></tr><tr><td><a href="https://case-studies.sismo.io">https://case-studies.sismo.io</a></td><td><a href=".gitbook/assets/Case Studies.png">Case Studies.png</a></td></tr></tbody></table>

## Case Study: Sybil-Resistant Airdrop from Privately Aggregated Data

SafeDrop is a Sybil-resistant and privacy-preserving ERC20 airdrop that distributes AIR tokens to users proportionally based on their reputation, aggregated from diverse sources of data (EVM wallets, Telegram, Twitter and GitHub accounts).

<figure><img src=".gitbook/assets/SafeDrop_Case Study (2).png" alt=""><figcaption></figcaption></figure>

By integrating Sismo Connect, SafeDrop users can generate a ZK proof to establish they own data in their accounts that make them eligible for the airdrop. \
No connection between these accounts and the airdrop destination address is ever made.

{% hint style="success" %}
Read the full case study [here](https://case-studies.sismo.io/db/safe-drop).
{% endhint %}

{% hint style="info" %}
Learn how to build the Sybil-resistant airdrop [here](build-with-sismo-connect/tutorials/tuto.md). Alternatively, check out the [onchain boilerplate](build-with-sismo-connect/run-example-apps/onchain-sample-project.md).
{% endhint %}
