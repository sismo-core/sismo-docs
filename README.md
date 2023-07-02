---
description: Sovereign Identity Aggregator and cryptonative SSO
cover: .gitbook/assets/GITBOOK_1900x400.png
coverY: 0
layout:
  cover:
    visible: true
    size: hero
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

# What Is Sismo?

Sismo leverages zero-knowledge proofs (ZKPs) and privacy-preserving technologies to **enable** **users to aggregate and selectively disclose personal data** to applications.&#x20;

By using Sismo Connect, an easy-to-integrate SSO, applications **can now safely obtain user data that was previously inaccessible.**

[Sismo Connect](welcome-to-sismo/what-is-sismo-connect.md) aims to replace non-sovereign SSOs such Google Connect and improve limited SSOs such as Wallet Connect.&#x20;

<figure><img src=".gitbook/assets/Introduction (1).png" alt=""><figcaption></figcaption></figure>

{% hint style="success" %}
Try out [demo apps](https://demo.apps.sismo.io/) on the Sismo App Store.
{% endhint %}

## Data Vault: Sovereign Identity Aggregator

Users aggregate their identity in their sovereign, local and private [Data Vault](how-sismo-works/technical-concepts/what-is-the-data-vault.md). They can now generate ZK Proofs from their personal data and leverage it across multiple platforms.

<figure><img src=".gitbook/assets/Aggregation (4).png" alt=""><figcaption></figcaption></figure>

Data Vaults contain Data Sources. Users generate ZK proofs to attest ownership of Data Sources. They can also reveal granular data by proving Data Sources are part in Data Groups. Data Groups can be created by anyone in the [Sismo Factory](https://factory.sismo.io).

Examples of Data Sources:

* Ethereum wallet
* GitHub, Twitter or Telegram account

Examples of Data Groups:

* Minters of  "[Stand with Crypto](https://nft.coinbase.com/collection/ethereum/0x9d90669665607f08005cae4a7098143f554c59ef)" NFT  ([Data Group](https://factory.sismo.io/groups-explorer?search=stand-with-crypto-nft-minters))
* Contributors to [Sismo Hub](https://github.com/sismo-core/sismo-hub) GitHub repo ([Data Group](https://factory.sismo.io/groups-explorer?search=sismo-hub-contributors-github))
* Gitcoin Passport Holders ([Data Group](https://factory.sismo.io/groups-explorer?search=gitcoin-passport-holders))
* [ENS DAO](https://docs.ens.domains/v/governance/) participants ([Data Group](https://factory.sismo.io/groups-explorer?search=ens-voters))
* Sismo Community Group ([Data Group](https://factory.sismo.io/groups-explorer?search=0xd630aa769278cacde879c5c0fe5d203c))

<figure><img src=".gitbook/assets/image.png" alt=""><figcaption><p>What a Data Vault looks like</p></figcaption></figure>

{% hint style="success" %}
You can create your own Data Vault and start aggregating your identity [here](https://vault-beta.sismo.io/).
{% endhint %}

## Prove & Verify: Selective Disclosure

Users participate in [proving schemes](how-sismo-works/core-components.md#what-are-proving-schemes) to make claims from their Data Sources.

{% hint style="info" %}
A proving scheme is a cryptographic method that allows one party (the prover) to prove to another party (the verifier) that a certain statement is true without revealing how it is true—ensuring privacy.
{% endhint %}

The Data Vault includes the provers, enabling users to generate zero-knowledge proofs (ZKPs) from their Data Sources (proof of ownership/ proof of inclusion in a Data Group). The ZKP is then verified by the application (in a smart contract for onchain apps, in a backend of offchain apps, in a browser/client for p2p apps)

Verifiers verify proofs from users and ensure their validity.

<figure><img src=".gitbook/assets/Selective Disclosure (1).png" alt=""><figcaption></figcaption></figure>

## Sismo Connect: The Crypto-Native SSO

Sismo Connect is a crypto-native single sign-on method (SSO) for onchain and offchain apps. Sismo Connect makes it easy for developers to request and verify ZK proofs attesting ownership/ group membership of their Data Sources.

<figure><img src=".gitbook/assets/Sismo Connect Flow (3).png" alt=""><figcaption></figcaption></figure>

{% hint style="success" %}
Discover applications leveraging the power of Sismo Connect on the [Sismo App Store](https://spaces.sismo.io/) and read the[ case studies](https://case-studies.sismo.io/) that we built around them.
{% endhint %}

Integration is simple with just a few lines of code: import the front-end package or React button for data requests, and verify proofs using Sismo’s Solidity or TypeScript package. Once integrated, applications can **request** private and granular data, while users can **authenticate** and **selectively disclose** their personal data.

{% hint style="info" %}
Learn how to integrate Sismo Connect into applications [here](broken-reference).
{% endhint %}
