---
description: The packages for Sismo Connect.
---

# Technical Documentation

[Sismo Connect](../../welcome-to-sismo/what-is-sismo-connect.md) is a crypto-native single sign-on method (SSO) for applications—whether on web2 or web3. Integration is simple with just a few lines of code: import the front-end package or React button for data requests, and verify proofs using Sismo Connect Solidity library in your smart contract OR server package in your backend. Once integrated, applications can **request** private and granular data, while users can **authenticate** and **selectively disclose** their personal data with the power of zero-knowledge proofs (ZKPs).

{% hint style="warning" %}
In order to use Sismo Connect, you will need to have an `appId` registered in the [Sismo Factory](https://factory.sismo.io/). [Here is a tutorial](../../sismo-factory/create-a-sismo-connect-app.md).
{% endhint %}

In the schema below, you can observe how Sismo Connect functions with both **onchain** and **offchain** applications:

<figure><img src="../../.gitbook/assets/Sismo Connect Flow.png" alt=""><figcaption><p>Sismo Connect Flow</p></figcaption></figure>

## **Packages**

* [`@sismo-core/sismo-connect-client`](https://github.com/sismo-core/sismo-connect-packages/tree/main/packages/sismo-connect-client): the frontend [package](client.md) to easily request ZKPs from users in a privacy-preserving manner.
* [`@sismo-core/sismo-connect-react`](https://github.com/sismo-core/sismo-connect-packages/tree/main/packages/sismo-connect-react): the React frontend [package](react.md) to easily integrate the sismoConnectButton and sismo-connect-client in your React app.
* [`@sismo-core/sismo-connect-server`](https://github.com/sismo-core/sismo-connect-packages/tree/main/packages/sismo-connect-server): the backend [package](server.md) to easily verify ZKPs offchain.
* [`@sismo-core/sismo-connect-solidity`](https://github.com/sismo-core/sismo-connect-packages/tree/main/packages/sismo-connect-solidity) : the [Solidity Library](solidity.md) to easily verify ZKPs onchain.

{% hint style="info" %}
Learn how to integrate Sismo Connect via these [tutorials](../tutorials/).&#x20;
{% endhint %}

## Glossary

### appId

The unique identifier of your Sismo Connect application created in the [Sismo Factory](../../sismo-factory/what-is-the-sismo-factory.md).

### Auth

An `Auth` is requested thanks to an `AuthRequest` to the [Data Vault ](../../#data-vault-aggregate-your-identity)in order to generate a proof of account ownership. It can be ownership of a SismoVault, an Ethereum address, a Twitter or GitHub account.

> Example: Prove that you own a Twitter account.

### Claim

A `Claim` is requested thanks to a `ClaimRequest` to the Data Vault in order to generate a group membership proof. It contains the groupId, the groupTimestamp, and the value you claim to have in the associated [Data Group](../../knowledge-base/resources/technical-concepts/data-gems-and-data-groups.md).

> Example: Prove that you are part of the latest generated "Gitcoin Passport Holders" Group with a minimum value of 15. The `groupId` of the group is [0x1cde61966decb8600dfd0749bd371f12](https://factory.sismo.io/groups-explorer?search=gitcoin-passport-holders).

### Signature

A `Signature` is requested thanks to an `SignatureRequest` to the Data Vault in order to embed a specific message in a generated proof. As far as this proof can't be verified without this exact message checked, we call this message a signature.

> Example: You are a DAO voting platform, and you want to get the vote of a user onchain. To do so, the user will create a signature containing a message (his vote) that will be used to generate the proof. When verifying the proof onchain, you are also verifying that the proof contains the message, it is then impossible for a malicious actor to take your proof and vote another thing with it.

### namespace

By default set to “main”. You can optionally define a `namespace` on top of the `appId` to use the Sismo Connect flow in different parts of your application. This is useful if you do not want users to generate the same proof for different services in your application. For example, if your application is a DAO voting website, you may define a different `namespace` for every voting campaign and use the `proofId` as a nullifier. Subsequently, users will be able to vote only once for each campaign as each proof request with a distinct `namespace` generates a distinct `proofId` for the same user. Learn more about Proof Identifiers [**here**](../../knowledge-base/resources/technical-concepts/vault-and-proof-identifiers.md).

### version

The version of the Data Vault app. The only version that currently works is `sismo-connect-v1`.

### groupId

The unique identifier of the group of accounts users prove membership in to generate a valid ZKP. [Data Groups](../../knowledge-base/resources/technical-concepts/data-gems-and-data-groups.md) are maintained by Sismo and can be created by anyone using the [Sismo Factory](https://factory.sismo.io/). They are composed of a name, a description, specifications, a timestamp, and data. Moreover, they are split into Group Snapshots which are timestamped.

### groupTimeStamp

Groups are composed of snapshots generated either once, daily, or weekly. Each Group Snapshot generated has an associated timestamp. By default, the selected group is the latest Group Snapshot generated. However, you are free to select a Group Snapshot with a timestamp other than the latest one.

### Vault Id and Proof Identifiers

The `userId` can contain different types of identifiers based on the `authType` requested. For instance, it can hold a Github Id for `AuthType.GITHUB`, a Twitter id for `AuthType.TWITTER`, an EVM address for `AuthType.EVM_ACCOUNT`, or a `vaultId` for `AuthType.VAULT`.

It's noteworthy that the `vaultId` is deterministically generated from the user's vault secret and the `appId` using a Poseidon hash. Therefore, if a user revisits your app, the `vaultId` remains consistent, but it will differ for other apps. This feature is beneficial if you wish to link a `vaultId` to a user in your database while preserving the user's privacy across various apps. You can delve deeper into the `vaultId` concept [here](../../knowledge-base/resources/technical-concepts/vault-and-proof-identifiers.md).

The `SismoConnectVerifiedResult`'s `verifiedClaim` also includes a `proofId`. This `proofId` is a unique identifier that represents a nullifier for a proof from Sismo. Similar to `vaultId`, the `proofId` is deterministically generated based on the `appId`, the `namespace`, the `groupId`, and the `groupTimestamp`. It enables you to verify if a user has previously submitted proof to your app for a specific namespace and group.

The `proofId` can be particularly handy if you want to organize private polls while ensuring each participant only vote once. For every poll, you can assign a distinct `namespace` such as `namespace: "my-poll-x"`, so a unique `proofId` is calculated for each instance. By storing the `proofId` and `vaultId` in your apps, you can guarantee that a user associated with a specific `vaultId` votes only once in each private poll. You can see a code example in the [**Sismo Connect Server package**](server.md).
