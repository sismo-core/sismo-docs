---
description: A simple privacy-preserving authentication system.
---

# Authenticate your users with zkConnect

### What's inside?&#x20;

This tutorial will walk you through a simple integration of [**zkConnect**](../../what-is-sismo/zkconnect.md) as a way of privately authenticating users in your application. This tutorial does not need any group to work, just an application Id (appId) created in the [**Sismo Factory**](https://factory.sismo.io/apps-explorer). You will only ask your users to prove ownership of a [Data Vault](../../what-is-sismo/data-vault.md).

{% hint style="info" %}
You can access an open-source repository implementing zkConnect:

* In React [here](https://github.com/sismo-core/zksub)
* In Next.js [here](https://github.com/sismo-core/zksub-next)

These 2 repositories use the [`@sismo-core/zk-connect-react`](https://docs.sismo.io/sismo-docs/technical-documentation/zkconnect/zkconnect-react-request)`package to request the proof, to see a full example of the` [`@sismo-core/zk-connect-client`](../../technical-documentation/zkconnect/zkconnect-client-request.md)`package, you can see` [`this branch`](https://github.com/sismo-core/zksub/tree/zk-connect-client-package) `of the React repository.`
{% endhint %}

### Choose your stack (Next.js recommended)

To implement zkConnect, you'll need both a frontend and a backend. The frontend will request the proof, and the backend will verify it.

For this tutorial, we recommend using the Next.js stack, which is a full-stack React framework. Next.js also offers a deployment service called Vercel, which makes it easy to deploy your app in just two clicks.&#x20;

To create a new Next.js project, run the following command:

```
yarn create next-app --typescript
```

That's it! Your frontend will be located in `src/pages/index.tsx`, and your backend will be located in `src/pages/api`.

You can find the complete example of a Next.js repository setup with zkConnect [here](https://github.com/sismo-core/zksub-next).

### Register your zkConnect App in the Factory

Before you begin integrating [**zkConnect**](../../what-is-sismo/zkconnect.md), you must register first a zkConnect app in the [**Sismo Factory**](https://factory.sismo.io/apps-explorer). This step is mandatory to obtain an application Id (`appId`), which is required during the zkConnect development process.

<details>

<summary>Why is an <code>appId</code> mandatory for zkConnect?</summary>

The `appId` will be used to compute the Vault Identifier, which is the the unique identifier for a user on your app. The Vault Identifier is simply the hash of a user's Vault secret and the appId.

$$vaultId = hash(vaultSecret, appId)$$

If we remove the appId from this simple calculation, we would have had the same vaultIdentifier for the same vaultSecret, effectively leaking information about a user that uses zkConnect on two different apps. The vaultId would be the same across different apps, and the user could be tracked if Vault Identifiers became public.

By introducing an appId, the vaultId is now different between apps, and the same user will have two different Vault IDs on two different apps, effectively preserving the user's privacy.&#x20;

You can learn more about this notion in the [Vault & Proof Identifiers article](../../technical-concepts/vault-and-proof-identifiers.md).

</details>

<figure><img src="../../.gitbook/assets/Capture d’écran 2023-03-14 à 19.47.52.png" alt=""><figcaption><p>Register your zkConnect App in the <a href="https://factory.sismo.io/apps-explorer">Sismo Factory</a></p></figcaption></figure>

You can register a zkConnect app here: [https://factory.sismo.io/apps-explorer](https://factory.sismo.io/apps-explorer).\
\
To create a zkConnect app, you need to log in with Sign-In With Ethereum and click on “create a new zkConnect app”. You will need to register an App Name, enter a description, and upload a logo alongside registering authorized domains. Pay attention to authorized domains, as these are the urls where the appId that will be created can be used for [zkConnect](../../technical-documentation/zkconnect/).&#x20;

{% hint style="info" %}
Feel free to add `*.com` to authorized domains when following along this tutorial. This will allow to whitelist `localhost`.
{% endhint %}

Once created, you should have all information about your app displayed in your profile:

<figure><img src="../../.gitbook/assets/Capture d’écran 2023-03-16 à 10.31.46.png" alt=""><figcaption><p>My zkConnect App</p></figcaption></figure>

The `appId` displayed on your app’s profile in the Factory is its unique identifier. You will use it to request proof from your users in your app’s front end and to verify it in the back end.

For this guide, you can use your own `appId`, such as `0x0ad03e347304dd6c19d9aa75db8659d9`.

### Request Vault Authentication from your users

Now that you have an appId registered for your application, you need to redirect your users to the Data Vault app to generate the required proof of Data Vault ownership. When redirected, users will be able to privately create a proof of ownership of their Data Vault before sending it to you.

To do that you will need to use one of our packages:

* Javascript / Typescript: [`@sismo-core/zk-connect-client`](../../technical-documentation/zkconnect/zkconnect-client-request.md)
* React: [`@sismo-core/zk-connect-react`](https://docs.sismo.io/sismo-docs/technical-documentation/zkconnect/zkconnect-react-request)``

{% tabs %}
{% tab title="React / Next.js" %}
First, you will need to import the following:

<pre class="language-bash"><code class="lang-bash"><strong>yarn add @sismo-core/zk-connect-react
</strong></code></pre>

\
After importing, you will be able to use the zkConnect button in your app.

<figure><img src="../../.gitbook/assets/zkConnect (2).png" alt=""><figcaption><p>zkConnect button</p></figcaption></figure>



To do so, you have to use the `ZkConnectButton` component:

<pre class="language-typescript"><code class="lang-typescript">import { ZkConnectButton, ZkConnectResponse } from "@sismo-core/zk-connect-react";

&#x3C;ZkConnectButton 
  appId={"0x0ad03e347304dd6c19d9aa75db8659d9"} // appId you registered
<strong>  onResponse={async (zkConnectResponse: ZkConnectResponse) => {
</strong>    //Send the response to your server to verify it
    //thanks to the @sismo-core/zk-connect-server package
    //Will see how to do this in next part of this tutorial
  }}
/>
</code></pre>



By clicking on this button your user will be redirected to the [Data Vault App](https://docs.sismo.io/sismo-docs/technical-documentation/data-vault-app), to generate a proof of Data Vault ownership. After your user generates the proof, he will be automatically redirected back to your app.



The `onResponse` props will allow you to get the response containing the proof that certifies the user has a Sismo Data Vault.
{% endtab %}

{% tab title="Javascript / Typescript" %}
First, you will need to import the following:

```bash
yarn add @sismo-core/zk-connect-client
```



After importing, the first step is to create a `zkConnectConfig` for your client and initialize a new ZKConnect client instance with it:

```typescript
// in your frontend
import { ZkConnect, ZkConnectClientConfig } from "@sismo-core/zk-connect-client";

const zkConnectConfig: ZkConnectClientConfig = {
  appId: "0x0ad03e347304dd6c19d9aa75db8659d9", // appId you registered
}
const zkConnect = new ZkConnect(zkConnectConfig);
```

You can then redirect your users to the [Data Vault App](../../technical-documentation/data-vault-app.md) to generate a proof of Data Vault ownership with the `request` function.

```typescript
// The `request` function sends your user to the Sismo Data Vault App 
// to generate the proof of Data Vault ownerhsip.
zkConnect.request();
```

After your user is done generating the proof in its Data Vault, he will be redirected to your app. You need to receive the generated proof via `getResponse` .

```tsx
const zkConnectResponse = zkConnect.getResponse();
```

This will return you a `ZkConnectResponse` which contains the user proof which certifies the user has a Sismo Data Vault.
{% endtab %}
{% endtabs %}

Well done! You’re halfway there! 💪

Now that you have the proof, you need to verify it in your backend to be sure that the proof is valid. If yes, you will allow your user to access your application.

### Verify the Auth Proof in your backend

A general rule in web development is that frontend input cannot be trusted.

Once you have obtained the proof from the front end, you should send it to the back end for a cryptographic verification.

To do that, you will need to use the [**`@sismo-core/zk-connect-server`**](../../technical-documentation/zkconnect/zkconnect-server-verify-off-chain.md) package.

First, you will need to import it:

```bash
yarn add @sismo-core/zk-connect-server
```

After importing, the first step is to create a `zkConnectConfig` for your server and initialize a new ZKConnect server instance with it:

```typescript
// in your backend
import { ZkConnect, ZkConnectServerConfig } from "@sismo-core/zk-connect-server";

const zkConnectConfig: ZkConnectServerConfig = {
  appId: "0x8f347ca31790557391cec39b06f02dc2", // appId you registered
}
const zkConnect = ZkConnect(zkConnectConfig);
```

With this ZkConnect instance, you can call the verify function which takes the `zkConnectResponse` sent by the frontend:

```typescript
const { vaultId } = await zkConnect.verify(zkConnectResponse);
```

If the proof is valid, a vaultId is returned. If not, an error is received.

The `vaultId` corresponds to a unique identifier for the user Data Vault, the best part of it is that this `vaultId` is derived from the `appId` so it is impossible to compare two vault ids from two different apps implementing zkConnect. **The vaultId can be used in your application as a user identifier**.

A user has now the ability to privately authenticate himself in any application integrating zkConnect by proving that he owns a Data Vault. 🤘

### Deploy your app (optional)

If you chose to use Next.js, we recommended using the Vercel service for deployment. With Vercel you will deploy your frontend and your backend in two clicks.&#x20;

Here's how to get started:

1. Create an account on [Vercel](https://vercel.com/) with your Github account
2. Create a new project and import your zksub repository in Vercel

<figure><img src="../../.gitbook/assets/Screenshot 2023-03-21 at 10.00.30.png" alt=""><figcaption><p>Import a Git repository</p></figcaption></figure>

3. When your repository is linked to Vercel, you should see this page appear:

<figure><img src="../../.gitbook/assets/Screenshot 2023-03-21 at 10.38.32.png" alt=""><figcaption><p>Configure your project</p></figcaption></figure>

Click on "Deploy", and congratulations! Your app is now deployed at https://\[Name of your app].vercel.app. 💪

### **Next steps**

All Data can be categorized into groups. If you liked this tutorial, you can see how to leverage zkConnect integration by asking for proof of user group membership in a privacy-preserving way with [this one](zk-connect-guide.md).&#x20;

If you have any questions about integrating zkConnect, don’t hesitate to reach out. The team will be happy to answer any questions you may have.

Get involved in the Sismo community:

* Look out for [**hackathons**](https://www.notion.so/sismo/Sismo-x-ETHPorto-2023-cbda827ea5f2469aa5fdbb4955fc18d6?pvs=4) that we are participating in
* Join our [**Discord**](https://discord.gg/sismo) **** or our [**Dev** **Telegram**](https://t.me/+Z-SwcvXZFRVhZTQ0)****
* See the [**Sismo-hub contributing page**](https://github.com/sismo-core/sismo-hub/issues)****
