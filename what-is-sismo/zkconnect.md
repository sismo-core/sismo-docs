---
description: The sovereign SSO
---

# zkConnect

Web3 gives users sovereign account ownership, yet data remains segregated on isolated sources. Furthermore, connecting to an application with a wallet address exposes all its associated data. These caveats create privacy concerns for users and limit the data applications can request.

zkConnect is a privacy-preserving single sign-on method for applications—whether on web2 or web3. Once integrated, applications can request private, granular data from users, while users can authenticate and selectively reveal their data.

{% hint style="info" %}
Applications may require just a fraction of a user’s data (e.g, being part of the PoH register) or data from multiple accounts (e.g, data from an Ethereum address and Twitter account) for access control or reputation importation.
{% endhint %}

## A privacy-preserving single sign-on method

Zero-knowledge proofs (ZKPs) allow users to selectively reveal valuable pieces of social capital in a privacy-preserving manner. These pieces of data, characterized as Data Gems, can be brought to applications—enabling users to reveal curated elements of their identities. However, zero-knowledge technology remains largely inaccessible to many developers.

By integrating zkConnect, developers can quickly implement zero-knowledge technology in their applications. zkConnect acts as the private communication layer between a user’s aggregated digital identity, stored in the Data Vault, and backend verifiers (either on-chain or off-chain). As a result, users can utilize the entirety of their data via a single privacy-preserving sign-on method when connecting to applications.

Through zkConnect, users can execute either or both of the following functions:

* Authenticate ownership of a unique Vault ID, an anonymous app-specific identifier functioning as a single sign-on method
* Selectively reveal pieces of personal data (Data Gems) to applications

{% hint style="info" %}
Developers can use zkConnect on their applications by integrating the associated frontend, backend, and Solidity packages. Read the full tutorial [here](../tutorials/zkconnect/zk-connect-guide.md).
{% endhint %}

## On-chain verification (coming soon)

After integrating the zkConnect Solidity Library, built on top of Sismo’s on-chain verifiers, in-app smart contracts can verify statements (e.g, a user claims to own a specific NFT) and accept them for access control or reputation importation. In this sense, users combine aspects of their data (Data Shards) to create verifiable statements (Data Gems). These Gems are verified by on-chain applications—importing valuable elements of their owner’s identity in the process.

Integrating zkConnect unlocks a privacy-preserving method of accessing on-chain applications or services. After clicking an integrated button, users are redirected to their Data Vault—where they generate a ZKP that an on-chain verifier subsequently authenticates. The entire experience requires minimal input from the user. Alternatively, it is possible to use zkConnect to mint Badges, which can likewise be utilized as an access tool.

[zkdrop.io](http://zkdrop.io/) is an on-chain application that facilitates privacy-preserving airdrops via zkConnect. Only owners of eligible accounts can claim airdrops on the app. Users click an integrated zkConnect button to prove ownership of an eligible account and receive the airdrop on a separate account. No link between the two accounts is ever made.

## Off-chain verification (beta)

In addition, off-chain applications can make use of zkConnect. While an on-chain application integrates Sismo’s verifier into a smart contract, an off-chain application must integrate Sismo’s verifier code into its backend. If an off-chain application limits its services to a single verifiable statement, it can also store Sismo’s anonymous app-specific Vault Identifier.

If an off-chain application has already integrated Sign-In with Ethereum (SIWE), zkConnect can enhance its existing infrastructure. By integrating Sismo’s verifier code, the application can accept verifiable statements (Data Gems) from users directly—adding a privacy-preserving layer to SIWE. Furthermore, off-chain applications can use Badges, the tokenized equivalents of Data Gems, as an access tool.

[zksub.io](https://www.zksub.io) is an example of an off-chain application using zkConnect. Currently, it enables The Merge contributors to register their email addresses in a privacy-preserving manner—gaining access to exclusive tickets for upcoming web3 events. zkConnect acts as a ZK layer between a contributor’s email address and private account.

