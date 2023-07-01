---
description: Making your data sovereign.
---

# What Is Sismo?

Sismo leverages zero-knowledge proofs (ZKPs) and privacy-preserving technologies to **enable** **users to aggregate and selectively disclose personal data** to applications.&#x20;

By using Sismo Connect, an easy-to-integrate SSO, applications **can now obtain users data that was previously inaccessible,** while respecting users' privacy.

[Sismo Connect](welcome-to-sismo/what-is-sismo-connect.md) aims to replace non-sovereign SSOs such Google Connect and improve limited SSOs such as Wallet Connect.&#x20;

{% hint style="success" %}
Try out [demo apps](https://demo.apps.sismo.io/) on the Sismo App Store.
{% endhint %}

## Sovereignty over our Identities

To be sovereign on the internet, we need both **ownership** over our digital identities and the **ability to aggregate them.**&#x20;

While blockchains made us owners of our data, Zero-Knowledge proofs - that enable private aggregation and selective disclosure - will give us true power over them. Together they provide sovereignty over one's digital identity.

With Sismo, users can finally leverage the entirety of their social capital at once, onchain and offchain. Smart contracts can access any personal data onchain, while backends no longer need to store more user information than necessary.

{% hint style="success" %}
With Sismo, you can:

* Aggregate your web2 and web3 data in a private Data Vault that you truly own.
* Selectively disclose your data to apps via Sismo Connect—the crypto-native SSO.
{% endhint %}

<figure><img src=".gitbook/assets/Introduction.png" alt=""><figcaption></figcaption></figure>

## Data Vault: Sovereign Identity Aggregator

Users aggregate their identity in their sovereign, local and private [Data Vault](how-sismo-works/technical-concepts/what-is-the-data-vault.md). By doing so, they can start generating ZK Proofs from their personal data and leverage it across multiple platforms.

<figure><img src=".gitbook/assets/Aggregation (5).png" alt=""><figcaption></figcaption></figure>

Data Vaults contain Data Sources. The granular data contained in Data Sources are called [Data Gems](how-sismo-works/core-components.md#what-are-data-gems-and-data-groups).  Users generate ZK proofs to attest ownership of Data Sources and Data Gems.

{% hint style="info" %}
Proving ownership of a Data Gem is effectively proving ownership + group membership of **a** Data Source in a Data Group.
{% endhint %}

Examples of Data Sources:

* Ethereum wallet
* GitHub, Twitter or Telegram account

Examples of Data Gems:

* Minter of  "[Stand with Crypto](https://nft.coinbase.com/collection/ethereum/0x9d90669665607f08005cae4a7098143f554c59ef)" NFT  ([Data Group](https://factory.sismo.io/groups-explorer?search=stand-with-crypto-nft-minters))
* Contributor to [Sismo Hub](https://github.com/sismo-core/sismo-hub) GitHub repo ([Data Group](https://factory.sismo.io/groups-explorer?search=sismo-hub-contributors-github))
* Gitcoin Passport Holders ([Data Group](https://factory.sismo.io/groups-explorer?search=gitcoin-passport-holders))
* [ENS DAO](https://docs.ens.domains/v/governance/) participant ([Data Group](https://factory.sismo.io/groups-explorer?search=ens-voters))

<figure><img src=".gitbook/assets/image.png" alt=""><figcaption><p>What a Data Vault looks like</p></figcaption></figure>

{% hint style="success" %}
You can create your own Data Vault and start aggregating your identity [here](https://vault-beta.sismo.io/).
{% endhint %}

## Prove & Verify: Selective Disclosure

Users participate in [proving schemes](how-sismo-works/core-components.md#what-are-proving-schemes) to prove ownership of a Data Sources/Gems. Proving ownership of a Data Gem is in fact proving ownership + group membership of **a** Data Source in a   [Data Group](how-sismo-works/technical-concepts/data-gems-and-data-groups.md).

New Data Groups can be easily created in the [Sismo Factory](https://factory.sismo.io/). This results in new Data Gems that can be selectively revealed to applications.

{% hint style="info" %}
A proving scheme is a cryptographic method that allows one party (the prover) to prove to another party (the verifier) that a certain statement is true without revealing how it is true—ensuring privacy.
{% endhint %}

The Data Vault includes the provers, enabling users to generate zero-knowledge proofs (ZKPs) that attest ownership of Data Sources/Gems. The ZKP is then verified by the application (in a smart contract for onchain apps, in a backend of offchain apps, in a browser/client for p2p apps)

Verifiers verify proofs from users and ensure their validity without accessing to the Data Source used to generate the ZK Proof. In this sense, users can selectively disclose Data Gems to applications without revealing the associated Data Source.

<figure><img src=".gitbook/assets/Selective Disclosure (2).png" alt=""><figcaption></figcaption></figure>

## Sismo Connect: The Crypto-Native SSO

Sismo Connect is a crypto-native single sign-on method (SSO) for onchain and offchain apps. Sismo Connect makes it easy for developers to request and verify ZK proofs attesting ownership of personal data (i.e. Data Sources and Data Gems).

<figure><img src=".gitbook/assets/Sismo Connect Flow (1).png" alt=""><figcaption></figcaption></figure>

{% hint style="success" %}
Discover applications leveraging the power of Sismo Connect on the [Sismo App Store](https://spaces.sismo.io/) and read the[ case studies](https://case-studies.sismo.io/) that we built around them.
{% endhint %}

Integration is simple with just a few lines of code: import the front-end package or React button for data requests, and verify proofs using Sismo’s Solidity or TypeScript package. Once integrated, applications can **request** private and granular data, while users can **authenticate** and **selectively disclose** their personal data.

{% hint style="info" %}
Learn how to integrate Sismo Connect into applications [here](broken-reference).
{% endhint %}
