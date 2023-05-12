---
description: Explore Sismo Connect's schema and functions.
---

# Empower Your App

[Sismo Connect](../#sismo-connect-the-crypto-native-sso) offers a unique approach to enhancing your application by facilitating secure and efficient communication between user data and your app. Unlike traditional Single Sign-On (SSO) systems, Sismo Connect is a versatile tool that can be integrated into both onchain and offchain applications, ensuring increased access to data derived from both web2 and web3 sources.

The schema below illustrates how Sismo Connect facilitates communication between a user’s personal data and applications:

<figure><img src="../.gitbook/assets/sismo-connect-2 (1).png" alt=""><figcaption></figcaption></figure>

## Unlock the Power of Sismo Connect: Functions

Using Sismo Connect, developers can implement any combination of the following functions in their applications:

* **Authenticate users**: Verify a user’s identity using the contents of their Data Vault. Once authenticated, Sismo Connect sends the necessary information to your application, granting them access without requiring additional logins.
* **Request data from users**: Privately request granular data from a user’s Data Vault. Users maintain full control throughout the process, with the power to selectively disclose their data to your application. Leverage this data on your application to enable access control and import a user’s reputation.
* **Request signatures from users**: Securely request and obtain user-signed messages, enabling trustless interactions, smart contract execution, and verifiable user authorization.

### Onchain Example — zkDrop

zkDrop is an onchain application that facilitates privacy-preserving airdrops via Sismo Connect. Users prove ownership of an eligible account and receive the airdrop on a separate account. No link between the two accounts is ever made.

<figure><img src="../.gitbook/assets/zkDrop_Gnenerate_ZK Proof_Enabeled.jpg" alt=""><figcaption></figcaption></figure>

zkDrop **authenticates** that users own a unique [Vault ID ](../knowledge-base/resources/technical-concepts/vault-and-proof-identifiers.md)(an anonymous identifier used to avoid double spends), **requests data** proving that users contributed to the Ethereum Merge and **requests a signature** from the recipient airdrop address to interact with an onchain smart contract.

{% hint style="info" %}
Learn how to integrate Sismo Connect into onchain applications [here](../build-with-sismo-connect/tutorials/onchain-tutorials/).
{% endhint %}

### Offchain Example — zkSub

zkSub is an example of an offchain application using Sismo Connect. Currently, it enables The Merge contributors to register their email addresses in a privacy-preserving manner—gaining access to exclusive tickets for upcoming web3 events. Sismo Connect acts as a ZK layer between a contributor’s email address and private account.

<figure><img src="../.gitbook/assets/zkSub_Gnenerate_ZK Proof_Enabeled.jpg" alt=""><figcaption></figcaption></figure>

zkSub **authenticates** that users own a unique Vault ID, **requests data** proving that users contributed to the Ethereum Merge and **requests a signature** to interact with an offchain back-end database.

{% hint style="info" %}
Learn how to integrate Sismo Connect into offchain applications [here](../build-with-sismo-connect/tutorials/offchain-tutorials.md).
{% endhint %}
