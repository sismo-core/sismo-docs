---
description: Get the most out of your data.
---

# Sismo: A Communication Protocol

Sismo is a communication protocol that enables users to selectively disclose personal data to applications.

The rapidly expanding ecosystem of applications on both web2 and web3 has made accounts and personal data synonymous with a user’s digital identity. Users build their identities as they accumulate data on their accounts, yet it remains segregated on isolated sources. Furthermore, connecting to an application with a wallet address exposes all its associated data. These caveats create privacy concerns for users and limit the data applications can request.

Though web3 empowers individuals to control their data, there is a pressing need for tools that enable users to harness their personal data without compromising privacy. By bridging the gap between personal data and applications, Sismo empowers users to take control of their digital identities and get the most out of their data.

## Prove & Verify

On a fundamental level, the Sismo communication protocol allows users to participate in [proving schemes](knowledge-base/resources/technical-concepts/proving-schemes/) that establish ownership of personal data. A proving scheme is a cryptographic approach in zero-knowledge technology that lets one party establish a statement's truth without revealing _how_ it is true. This is achieved through communication between provers, that generate proofs in a user’s [Data Vault](what-is-sismo/personal-data-sismos-data-vault-gems-and-groups.md) (an aggregator of personal data), and verifiers, that validate these proofs when integrated into applications via Sismo Connect.

Provers enable individuals to generate zero-knowledge proofs (ZKPs) that attest ownership of valuable personal data. After receiving a request, a user generates a proof to make a claim about specific pieces of data that they own—which can be subsequently verified. These valuable pieces of atomic data, which can be revealed without exposing the associated source of truth, are characterized as [Data Gems](what-is-sismo/personal-data-sismos-data-vault-gems-and-groups.md#data-gems-and-data-groups).

In turn, verifiers accept proofs from users and ensure their validity—whether in onchain smart contracts or offchain databases. As a result, users can selectively disclose their data while applications access the information they need to provide personalized experiences without compromising user privacy.

## Sismo Connect: The Crypto-Native SSO

Sismo Connect is a crypto-native single sign-on method (SSO) for applications—whether on web2 or web3. Integration is simple with just a few lines of code: import the front-end package or React button for data requests, and verify proofs using Sismo’s Solidity or TypeScript package. Once integrated, applications can **request** private and granular data, while users can **authenticate** and **selectively disclose** their personal data.

Applications may require just a fraction of a user’s data or data from multiple accounts for access control or reputation importation. In the example below, zkDrop airdrops an NFT to users that own a Gitcoin Passport and Nouns NFT on public and private wallets.

<figure><img src=".gitbook/assets/Screenshot 2023-05-11 at 18.28.31.png" alt=""><figcaption></figcaption></figure>

Sismo’s communication protocol, with the power of ZKPs, allows users to selectively disclose valuable pieces of data in a privacy-preserving manner. These pieces of data, characterized as Data Gems, can be brought to applications—enabling users to reveal curated elements of their identities. Consequently, Sismo Connect puts users in complete control of their data while enhancing data-driven, crypto-native applications.

{% hint style="info" %}
Discover applications leveraging the power of Sismo Connect on [Sismo Spaces](https://spaces.sismo.io/).
{% endhint %}

By integrating Sismo Connect, developers can quickly implement zero-knowledge technology in their applications. Sismo Connect is the developer-centered method of leveraging Sismo’s communication protocol to facilitate a connection between a user’s aggregated digital identity, stored in the Data Vault, and backend verifiers (either onchain or offchain).

Through Sismo Connect, developers can utilize any or a combination of the following functions:

* Authenticate users via the contents of their Data Vault
* Request granular data (Data Gems) from users
* Request users to sign messages

{% hint style="info" %}
Learn how to integrate Sismo Connect into applications [here](build-with-sismo-connect/overview.md).
{% endhint %}

## The Team Behind Sismo

At the heart of Sismo lies a tight-knit team of passionate individuals who share a common vision: reshaping the way personal data and applications communicate. The team is based primarily in Paris, France—with additional personnel across the world.

Sismo's core values revolve around innovation, privacy and empowering users and applications alike. The team firmly believes in giving individuals the tools they need to maintain control over their data. By pushing the boundaries of what's possible on both web2 and web3, Sismo is paving the way for a new era of communication between personal data and applications.
