# Technical Documentation

[Sismo Connect](../../#sismo-connect-the-crypto-native-sso) is a crypto-native single sign-on method (SSO) for applications—whether on web2 or web3. Integration is simple with just a few lines of code: import the front-end package or React button for data requests, and verify proofs using the Sismo Connect Solidity library in your smart contract OR server package in your back end. Once integrated, applications can **request** private and granular data, while users can **authenticate** and **selectively disclose** their personal data with the power of zero-knowledge proofs (ZKPs).

{% hint style="warning" %}
In order to use Sismo Connect, you will need to have an `appId` registered in the [Sismo Factory](https://factory.sismo.io/). [Here is a tutorial](../tutorials/create-a-sismo-connect-app.md).
{% endhint %}

In the schema below, you can observe how Sismo Connect functions with both **onchain** and **offchain** applications:

<figure><img src="../../.gitbook/assets/Sismo Connect Flow (2) (1).png" alt=""><figcaption><p>Sismo Connect Flow</p></figcaption></figure>

### Sismo Connect Flow Walkthrough

1. The Sismo Connect app requests proofs from its users by integrating the Sismo Connect React button from [`@sismo-core/sismo-connect-react`](https://github.com/sismo-core/sismo-connect-packages/tree/main/packages/sismo-connect-react) or using the [`@sismo-core/sismo-connect-client`](https://github.com/sismo-core/sismo-connect-packages/tree/main/packages/sismo-connect-client)package. Users are redirected to their Data Vault to generate proofs.
2. After successful proof generation, users are redirected back to the Sismo Connect app with their generated proofs. The app's front end can either call a smart contract or its back end to verify the proofs.
3. The proof is received by the smart contract OR the backend and verified.
4. Certain logic is then executed if proofs are valid, successfully ending the Sismo Connect flow.

{% hint style="info" %}
Learn how to integrate Sismo Connect via these [**tutorials**](../tutorials/) or gain more in-depth knowledge of Sismo Connect by learning what is a [**Sismo Connect Configuration**](sismo-connect-configuration.md), an [**Auth**](auths.md), a [**Claim**](claims.md) and a [**Signature**](signature.md).
{% endhint %}

### **Packages**

Here are the different GitHub links to Sismo Connect packages. You can find their documentation [**here**](packages/).

* [`@sismo-core/sismo-connect-client`](https://github.com/sismo-core/sismo-connect-packages/tree/main/packages/sismo-connect-client): the front-end [package](packages/client.md) to easily request ZKPs from users in a privacy-preserving manner.
* [`@sismo-core/sismo-connect-react`](https://github.com/sismo-core/sismo-connect-packages/tree/main/packages/sismo-connect-react): the React front-end [package](packages/react.md) to easily integrate the sismoConnectButton and sismo-connect-client in your React app.
* [`@sismo-core/sismo-connect-server`](https://github.com/sismo-core/sismo-connect-packages/tree/main/packages/sismo-connect-server): the back-end [package](packages/server.md) to easily verify ZKPs offchain.
* [`@sismo-core/sismo-connect-solidity`](https://github.com/sismo-core/sismo-connect-solidity) : the [Solidity Library](packages/solidity.md) to easily verify ZKPs onchain.
