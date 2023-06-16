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

* [`@sismo-core/sismo-connect-client`](https://github.com/sismo-core/sismo-connect-packages/tree/main/packages/sismo-connect-client): the frontend [package](packages/client.md) to easily request ZKPs from users in a privacy-preserving manner.
* [`@sismo-core/sismo-connect-react`](https://github.com/sismo-core/sismo-connect-packages/tree/main/packages/sismo-connect-react): the React frontend [package](packages/react.md) to easily integrate the sismoConnectButton and sismo-connect-client in your React app.
* [`@sismo-core/sismo-connect-server`](https://github.com/sismo-core/sismo-connect-packages/tree/main/packages/sismo-connect-server): the backend [package](packages/server.md) to easily verify ZKPs offchain.
* [`@sismo-core/sismo-connect-solidity`](https://github.com/sismo-core/sismo-connect-packages/tree/main/packages/sismo-connect-solidity) : the [Solidity Library](packages/solidity.md) to easily verify ZKPs onchain.

{% hint style="info" %}
Learn how to integrate Sismo Connect via these [tutorials](../tutorials/).&#x20;
{% endhint %}
