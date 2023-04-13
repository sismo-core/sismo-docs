---
description: The crypto-native SSO.
---

# sismoConnect

Web3 gives users sovereign account ownership, yet data remains segregated on isolated sources. Furthermore, connecting to an application with a wallet address exposes all its associated data. These caveats create privacy concerns for users and limit the data applications can request.

sismoConnect is a crypto-native single sign-on method for applications—whether on web2 or web3. Once integrated, applications can request private, granular data from users, while users can authenticate and selectively reveal their data. Applications may require just a fraction of a user’s data (e.g, being part of the PoH register) or data from multiple accounts (e.g, data from an Ethereum address and Twitter account) for access control or reputation importation.

{% hint style="info" %}
Learn more about sismoConnect through the associated [technical documentation](../technical-documentation/sismo-connect/) and [tutorials](../tutorials/sismo-connect/). sismoConnect is currently in beta and may be subject to change.
{% endhint %}

## A privacy-preserving single sign-on method

Zero-knowledge proofs (ZKPs) allow users to selectively reveal valuable pieces of social capital in a privacy-preserving manner. These pieces of data, characterized as Data Gems, can be brought to applications—enabling users to reveal curated elements of their identities. However, zero-knowledge technology remains largely inaccessible to many developers.

By integrating sismoConnect, developers can quickly implement zero-knowledge technology in their applications. sismoConnect acts as the private communication layer between a user’s aggregated digital identity, stored in the Data Vault, and backend verifiers (either on-chain or off-chain). As a result, users can utilize the entirety of their data via a single privacy-preserving sign-on method when connecting to applications.

Through sismoConnect, developers can:

* Authenticate users
* Request granular data (Data Gems) from users
* Request users to sign messages

## On-chain verification

After integrating the [sismoConnect Solidity Library](../technical-documentation/sismo-connect/solidity-library.md), built on top of Sismo’s on-chain verifiers, in-app smart contracts can verify user claims (e.g, a user claims to own a specific NFT) and accept them for access control or reputation importation. Users make claims about their data and generate proofs to verify ownership of valuable social capital. Ownership is verified by on-chain applications—importing valuable elements of their owner’s identity in the process.

Integrating sismoConnect unlocks a privacy-preserving method of accessing on-chain applications or services. After clicking an integrated button, users are redirected to their Data Vault—where they generate a ZKP that an on-chain verifier subsequently authenticates. The entire experience requires minimal input from the user. Alternatively, it is possible to use sismoConnect to mint Badges, which can likewise be utilized as an access tool.

{% hint style="info" %}
Learn how to integrate sismoConnect into an on-chain application via this [tutorial](../tutorials/sismo-connect/gate-your-contracts-with-sismo-connect-advanced.md). Alternatively, see the full technical documentation [here](../technical-documentation/sismo-connect/).
{% endhint %}

[zkdrop.io](http://zkdrop.io/) is an on-chain application that facilitates privacy-preserving airdrops via sismoConnect. Only owners of eligible accounts can claim airdrops on the app. Users click an integrated sismoConnect button to prove ownership of an eligible account and receive the airdrop on a separate account. No link between the two accounts is ever made.

{% hint style="success" %}
Try out a demo [here](https://demo.zkdrop.io/mergooor-pass). You can also find an open-source GitHub repository [here](https://github.com/sismo-core/zkdrop).
{% endhint %}

## Off-chain verification

In addition, off-chain applications can make use of sismoConnect. While an on-chain application integrates Sismo’s verifier into a smart contract, an off-chain application must integrate Sismo’s verifier code into its backend. If an off-chain application limits its services to a single verifiable claim, it can also store Sismo’s anonymous app-specific [AnonUserID](../technical-concepts/vault-and-proof-identifiers.md).

If an off-chain application has already integrated Sign-In with Ethereum (SIWE), sismoConnect can enhance its existing infrastructure. By integrating Sismo’s verifier code, the application can accept verifiable claims from users directly—adding a privacy-preserving layer to SIWE. Furthermore, off-chain applications can use Badges, the tokenized equivalents of Data Gems, as an access tool.

{% hint style="info" %}
Learn how to integrate sismoConnect into an off-chain application via this [tutorial](../tutorials/sismo-connect/request-data-privately-with-sismo-connect.md). Alternatively, see the full technical documentation [here](../technical-documentation/sismo-connect/).
{% endhint %}

[zksub.io](http://www.zksub.io) is an example of an off-chain application using sismoConnect. Currently, it enables The Merge contributors to register their email addresses in a privacy-preserving manner—gaining access to exclusive tickets for upcoming web3 events. sismoConnect acts as a ZK layer between a contributor’s email address and private account.

{% hint style="success" %}
Try out a demo [here](https://demo.zksub.io/). You can also find an open-source GitHub repository [here](https://github.com/sismo-core/zksub).
{% endhint %}
