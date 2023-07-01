---
description: The packages for Sismo Connect.
---

# Technical Documentation

[Sismo Connect](../../welcome-to-sismo/what-is-sismo-connect.md) is a crypto-native single sign-on method (SSO) for applications—whether on web2 or web3. Integration is simple with just a few lines of code: import the front-end package or React button for data requests, and verify proofs using Sismo Connect Solidity library in your smart contract OR server package in your backend. Once integrated, applications can **request** private and granular data, while users can **authenticate** and **selectively disclose** their personal data with the power of zero-knowledge proofs (ZKPs).

{% hint style="warning" %}
In order to use Sismo Connect, you will need to have an `appId` registered in the [Sismo Factory](https://factory.sismo.io/). [Here is a tutorial](../tutorials/create-a-sismo-connect-app.md).
{% endhint %}

In the schema below, you can observe how Sismo Connect functions with both **onchain** and **offchain** applications:

<figure><img src="../../.gitbook/assets/Sismo Connect Flow.png" alt=""><figcaption><p>Sismo Connect Flow</p></figcaption></figure>

### Sismo Connect Flow Walkthrough

1. The Sismo Connect App requests some proofs to its users by integrating the Sismo Connect React button from [`@sismo-core/sismo-connect-react`](https://github.com/sismo-core/sismo-connect-packages/tree/main/packages/sismo-connect-react) or using the [`@sismo-core/sismo-connect-client`](https://github.com/sismo-core/sismo-connect-packages/tree/main/packages/sismo-connect-client)package. Users are redirected to their Sismo Vault to generate proofs.
2. After successful proofs generation, users are redirected back to the Sismo Connect App with their generated proofs. The app frontend can either call a smart contract or its backend to verify the proofs.
3. The proof is received by the smart contract OR the backend and verified.
4. Some logic is then executed if proofs are valid, successfully ending the Sismo Connect flow.

{% hint style="info" %}
Learn how to integrate Sismo Connect via these [**tutorials**](../tutorials/) or gain more in-depth knowledge of Sismo Connect by learning what is a [**Sismo Connect Configuration**](sismo-connect-configuration.md), an [**Auth**](auths.md), a [**Claim**](claims.md) and a [**Signature**](signature.md).
{% endhint %}

### **Packages**

Here are the different GitHub links to Sismo Connect packages, you can find their documentation [**here**](packages/).

* [`@sismo-core/sismo-connect-client`](https://github.com/sismo-core/sismo-connect-packages/tree/main/packages/sismo-connect-client): the frontend [package](packages/client.md) to easily request ZKPs from users in a privacy-preserving manner.
* [`@sismo-core/sismo-connect-react`](https://github.com/sismo-core/sismo-connect-packages/tree/main/packages/sismo-connect-react): the React frontend [package](packages/react.md) to easily integrate the sismoConnectButton and sismo-connect-client in your React app.
* [`@sismo-core/sismo-connect-server`](https://github.com/sismo-core/sismo-connect-packages/tree/main/packages/sismo-connect-server): the backend [package](packages/server.md) to easily verify ZKPs offchain.
* [`@sismo-core/sismo-connect-solidity`](https://github.com/sismo-core/sismo-connect-packages/tree/main/packages/sismo-connect-solidity) : the [Solidity Library](packages/solidity.md) to easily verify ZKPs onchain.
