---
description: The packages for a sovereign SSO
---

# zkConnect

[zkConnect](../../what-is-sismo/zkconnect.md) is a privacy-preserving single sign-on method for applications. Once integrated, applications can request private, granular data from users, while users can authenticate and selectively reveal their data thanks to zero-knowledge proofs (ZKPs).

zkConnect features two packages:

* ``[`@sismo-core/zk-connect-client`](https://github.com/sismo-core/zk-connect-packages/tree/main/packages/zk-connect-client): the frontend package to easily request ZKPs from users of Sismo in a privacy-preserving manner.
* ``[`@sismo-core/zk-connect-react`](https://github.com/sismo-core/zk-connect-packages/tree/main/packages/zk-connect-react): the React frontend package to easily integrate the zkConnectButton and zk-connect-client in your React app.
* [`@sismo-core/zk-connect-server`](https://github.com/sismo-core/zk-connect-packages/blob/main/packages/zk-connect-server): the backend package to easily verify these ZKPs.

In order to use [zkConnect](../../what-is-sismo/zkconnect.md), you will need to have an `appId` registered in the [Sismo Factory](https://factory.sismo.io/apps-explorer). You can register your appId [here](https://factory.sismo.io/apps-explorer).

You can see this guide for a full example on how to integrate zkConnect in your application: [zkConnect Guide](../../tutorials/zkconnect/zk-connect-guide.md).
