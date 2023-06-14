---
description: Explore Sismo Connect's schema, functions and see real-world examples.
---

# Discover Sismo Connect

[Sismo Connect](../#sismo-connect-the-crypto-native-sso) is a crypto-native single sign-on (SSO) that enables applications to access data aggregated in a user’s [Data Vault](../#data-vault-aggregate-your-identity) without compromising their privacy. Integrating Sismo Connect equips developers with the means to implement Sismo's communication protocol in their applications, thereby facilitating the **aggregation** of personal data, all while preserving user **privacy** via selective disclosure. This balance fosters innovation and empowers users, paving the way for a new wave of data-driven applications—whether onchain or offchain.

The schema below illustrates how Sismo Connect facilitates communication between a user’s personal data, aggregated in the Data Vault, and applications:

<figure><img src="../.gitbook/assets/Sismo Connect Flow.png" alt=""><figcaption></figcaption></figure>

## Empower Your App

Sismo Connect unlocks access to a vast pool of user data for your application, derived from both web2 and web3. This enables innovative data-driven enhancements, from access control to reputation importation and personalized experiences. With Sismo Connect, your application respects user privacy, allowing for the responsible utilization of personal data. By enabling users to selectively disclose granular pieces of data, known as Data Gems, you eliminate the friction users experience when hesitating to reveal sensitive information.

Using Sismo Connect, you can implement any combination of the following functions in your application:

* **Authenticate users**: Verify a user’s identity using the Data Sources in their Data Vault. Once authenticated, Sismo Connect sends the necessary information to your application, granting them access without requiring additional logins.
* **Request data from users**: Privately request granular data, characterized as Data Gems, from a user’s Data Vault. Users maintain full control throughout the process, with the power to selectively disclose their data to your application. Leverage this data on your application to enable access control, reputation importation and other innovative data-driven experiences.
* **Request signatures from users**: Securely request and obtain user-signed messages, enabling trustless interactions, smart contract execution, and verifiable user authorization.

{% tabs %}
{% tab title="Onchain Example — zkDrop" %}
zkDrop is an onchain application that facilitates privacy-preserving airdrops via Sismo Connect. Users can prove eligibility for the airdrop using aggregated data from multiple unlinked accounts and receive the airdrop on a separate account. No link between the source accounts and the airdrop destination address is ever made.

zkDrop **authenticates** that users own a unique [Vault ID ](../knowledge-base/resources/technical-concepts/vault-and-proof-identifiers.md)(an anonymous identifier used to avoid double spends), **requests data** from multiple sources and **requests a signature** from the recipient airdrop address to interact with an onchain smart contract.

<figure><img src="../.gitbook/assets/zkDrop_Vault app_Gnenerate-ZK-Proof.jpg" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Learn how to integrate Sismo Connect into onchain applications [here](../build-with-sismo-connect/tutorials/onchain-tutorials/).
{% endhint %}
{% endtab %}

{% tab title="Offchain Example — zkSub" %}
zkSub is an example of an offchain application using Sismo Connect. It enables users to register their email addresses for certain services in a privacy-preserving manner. Users can prove eligibility for the service using aggregated data derived from multiple sources without creating links between them. Sismo Connect acts as a ZK layer between a contributor’s email address and private account.

zkSub **authenticates** that users own a unique Vault ID, **requests data** from multiple sources and **requests a signature** to interact with an offchain back-end database.

<figure><img src="../.gitbook/assets/zkSub_Vault app_Gnenerate-ZK-Proof.jpg" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Learn how to integrate Sismo Connect into offchain applications [here](../build-with-sismo-connect/tutorials/offchain-tutorials.md).
{% endhint %}
{% endtab %}
{% endtabs %}

## Data Vault in Action

When accessing an application via Sismo Connect, users selectively disclose the contents of their [Data Vault](../#data-vault-aggregate-your-identity).

Envision the Data Vault as an encrypted aggregator that securely stores various Data Sources, such as Ethereum addresses, GitHub, Twitter and Telegram accounts, and other attestations from relevant authorities.

<figure><img src="../.gitbook/assets/dv.png" alt=""><figcaption></figcaption></figure>

Once users have imported a Data Source, Sismo Connect enables them to prove ownership and selectively disclose information to applications without compromising privacy.

<figure><img src="../.gitbook/assets/The Merge Contributors Space.jpeg" alt=""><figcaption></figcaption></figure>

For instance, imagine a user has imported two Ethereum addresses: a public ENS domain and a private wallet owning a specific NFT. Sismo allows them to:

1. Prove ownership of their public wallet (Data Source)
2. Selectively disclose they own the NFT (Data Gem) without revealing their private wallet.

The Data Vault ensures users have full control over their aggregated digital identity, with the flexibility to authenticate and share information on their own terms.

{% hint style="info" %}
You can create your own Data Vault and start aggregating your identity [here](https://vault-beta.sismo.io/).
{% endhint %}

## Spaces for Your Community

[Spaces ](https://spaces.sismo.io/)serve as a platform for community builders to showcase applications integrated with Sismo Connect. Explore the following Spaces to see applications harnessing the power of Sismo Connect:

* [**Privacy Is Normal**](https://spaces.sismo.io/privacy-is-normal): This Space focuses on the importance of privacy as a fundamental human right. By proving ownership of a Gitcoin Passport and Tornado Cash user status, you can enter a Sybil-resistant lottery to win a printed tribute to Tornado Cash’s foundational code, or its NFT equivalent. This data can be **aggregated** from multiple sources and proven in a privacy-preserving manner through **selective disclosure**.

<figure><img src="../.gitbook/assets/Privacy in Normal Space.jpg" alt=""><figcaption></figcaption></figure>

* [**The Merge Contributors**](https://spaces.sismo.io/the-merge-contributors): This Space contains applications using the \[the-merge-contributor] Data Gem as an access control tool. If users prove ownership of an account that contributed to The Merge on Ethereum, they can receive exclusive access to web3 events via an onchain NFT or offchain mailing list. As users **selectively disclose** the Data Gem, they safeguard their privacy while proving eligibility.

<figure><img src="../.gitbook/assets/The Merge Contributors Space 2.jpg" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Visit [Sismo Spaces](https://spaces.sismo.io/) to explore applications that harness the power of Sismo Connect.
{% endhint %}
