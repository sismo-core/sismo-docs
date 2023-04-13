---
description: A simple privacy-preserving authentication system.
---

# Authenticate your users with sismoConnect

### What's inside?&#x20;

This tutorial will walk you through a simple integration of [**sismoConnect**](../../technical-documentation/sismo-connect/) as a way of privately authenticating users in your application. This tutorial does not need any group to work, just an application Id (appId) created in the [**Sismo Factory**](https://factory.sismo.io/apps-explorer). You will only ask your users to prove ownership of a [Data Vault](../../what-is-sismo/data-vault.md).

{% hint style="info" %}
You can access the open-source boilerplate repository showcasing multiple sismoConnect integrations in Next [here](https://github.com/sismo-core/sismo-connect-boilerplate).

The repository use the [`@sismo-core/sismo-connect-react`](../../technical-documentation/sismo-connect/react.md)package to request the proof, if your project is not on react you can use the[`@sismo-core/sismo-connect-client`](../../technical-documentation/sismo-connect/client.md)package`.`
{% endhint %}

### Choose your stack (Next.js recommended)

To implement sismoConnect, you'll need both a frontend and a backend. The frontend will request the proof, and the backend will verify it.

For this tutorial, we recommend using the Next.js stack, which is a full-stack React framework. Next.js also offers a deployment service called Vercel, which makes it easy to deploy your app in just two clicks.&#x20;

To create a new Next.js project, run the following command:

```
yarn create next-app --typescript
```

That's it! Your frontend will be located in `src/pages/index.tsx`, and your backend will be located in `src/pages/api`.

You can find the complete example of a Next.js repository setup with sismoConnect [here](https://github.com/sismo-core/sismo-connect-boilerplate).

### Register your sismoConnect App in the Factory

Before you begin integrating [**sismoConnect**](../../readme/sismo-connect.md), you must register first a sismoConnect app in the [**Sismo Factory**](https://factory.sismo.io/apps-explorer). This step is mandatory to obtain an application Id (`appId`), which is required during the sismoConnect development process.

<details>

<summary>Why is an <code>appId</code> mandatory for sismoConnect?</summary>

The `appId` will be used to compute an AnonUserID, which is the the unique identifier for a user on your app. The AnonUserID is simply the hash of a user's Vault secret and the appId.

$$vaultId = hash(vaultSecret, appId)$$

If we remove the appId from this simple calculation, we would have had the same AnonUserID for the same vaultSecret, effectively leaking information about a user that uses sismoConnect on two different apps. The AnonUserID would be the same across different apps, and the user could be tracked if the AnonUserIDs became public.

By introducing an appId, the vaultId is now different between apps, and the same user will have two different AnonUserIDs on two different apps, effectively preserving the user's privacy.&#x20;

You can learn more about this notion in this [article](../../technical-concepts/vault-and-proof-identifiers.md).

</details>

<figure><img src="../../.gitbook/assets/Capture d’écran 2023-04-12 à 11.26.24.png" alt=""><figcaption><p>Register your sismoConnect App in the <a href="https://factory.sismo.io/apps-explorer">Sismo Factory</a></p></figcaption></figure>

You can register a sismoConnect app here: [https://factory.sismo.io/apps-explorer](https://factory.sismo.io/apps-explorer).\
\
To create a sismoConnect app, you need to log in with Sign-In With Ethereum and click on “create a new sismoConnect app”. You will need to register an App Name, enter a description, and upload a logo alongside registering authorized domains. Pay attention to authorized domains, as these are the urls where the appId that will be created can be used for [sismoConnect](../../readme/sismo-connect.md).&#x20;

{% hint style="info" %}
Feel free to add `*.com` to authorized domains when following along this tutorial. This will allow to whitelist `localhost`.
{% endhint %}

Once created, you should have all information about your app displayed in your profile:

<figure><img src="../../.gitbook/assets/Capture d’écran 2023-04-12 à 11.28.37.png" alt=""><figcaption><p>My sismoConnect App</p></figcaption></figure>

The `appId` displayed on your app’s profile in the Factory is its unique identifier. You will use it to request proof from your users in your app’s front end and to verify it in the back end.

For this guide, you can use your own `appId`, such as `0x0ad03e347304dd6c19d9aa75db8659d9`.

### Request Vault Authentication from your users

Now that you have an appId registered for your application, you need to redirect your users to the Data Vault app to generate the required proof of Data Vault ownership. When redirected, users will be able to privately create a proof of ownership of their Data Vault before sending it to you.

To do that you will need to use one of our packages:

* Javascript / Typescript: [`@sismo-core/sismo-connect-client`](../../technical-documentation/sismo-connect/client.md)
* React: [`@sismo-core/sismo-connect-react`](../../technical-documentation/sismo-connect/react.md)

{% tabs %}
{% tab title="React / Next.js" %}
First, you will need to import the following:

<pre class="language-bash"><code class="lang-bash"><strong>yarn add @sismo-core/sismo-connect-react
</strong></code></pre>

After importing, you will be able to use the sismoConnect button in your app.

<figure><img src="../../.gitbook/assets/Capture d’écran 2023-04-12 à 11.30.56.png" alt=""><figcaption><p>sismoConnect React button</p></figcaption></figure>

To do so, you have to use the `SismoConnectButton` component:

```typescript
import { 
  AuthType,
  SismoConnectButton, 
  SismoConnectClientConfig, 
  SismoConnectResponse
} from "@sismo-core/sismo-connect-react";

const sismoConnectConfig: SismoConnectClientConfig = {
  appId: "0x0ad03e347304dd6c19d9aa75db8659d9" // appId you registered
};

<SismoConnectButton
  config={sismoConnectConfig} // the config created above
  // you ask for a proof of Data Vault ownership
  auths={[{authType:AuthType.VAULT}]} 
  onResponse={(response: SismoConnectResponse) => {
    //Send the response to your server to verify it
    //thanks to the @sismo-core/sismo-connect-server package
    //Will see how to do this in next part of this tutorial
  }}
/>
```

By clicking on this button your user will be redirected to the [Data Vault App](https://docs.sismo.io/sismo-docs/technical-documentation/data-vault-app), to generate a proof of Data Vault ownership. After your user generates the proof, he will be automatically redirected back to your app.

The `onResponse` props will allow you to get the response containing the proof that certifies the user has a Sismo Data Vault.

{% hint style="success" %}
You can view the complete frontend integration for a simple auth [here](https://github.com/sismo-core/sismo-connect-boilerplate/blob/main/src/pages/off-chain/simple-auth.tsx).
{% endhint %}
{% endtab %}

{% tab title="Javascript / Typescript" %}
First, you will need to import the following:

```bash
yarn add @sismo-core/sismo-connect-client
```

After importing, the first step is to create a `sismoConnectConfig` for your client and initialize a new ZKConnect client instance with it:

```typescript
// in your frontend
import { AuthType, SismoConnect, SismoConnectClientConfig } from "@sismo-core/zk-connect-client";

const sismoConnectConfig: SismoConnectClientConfig = {
  appId: "0x0ad03e347304dd6c19d9aa75db8659d9", // appId you registered
}
const sismoConnect = SismoConnect(sismoConnectConfig);
```

You can then redirect your users to the [Data Vault App](../../technical-documentation/data-vault-app.md) to generate a proof of Data Vault ownership with the `request` function.

```typescript
// The `request` function sends your user to the Sismo Data Vault App 
// to generate the proof of Data Vault ownerhsip.
sismoConnect.request({
    auths: [{authType: AuthType.VAULT}]
});
```

After your user is done generating the proof in its Data Vault, he will be redirected to your app. You need to receive the generated proof via `getResponse` .

```tsx
const sismoConnectResponse = sismoConnect.getResponse();
```

This will return you a `SismoConnectResponse` which contains the user proof which certifies the user has a Sismo Data Vault.
{% endtab %}
{% endtabs %}

Well done! You’re halfway there! 💪

Now that you have the proof, you need to verify it in your backend to be sure that the proof is valid. If yes, you will allow your user to access your application.

### Verify the Auth Proof in your backend

A general rule in web development is that frontend input cannot be trusted.

Once you have obtained the proof from the front end, you should send it to the back end for a cryptographic verification.

To do that, you will need to use the [**`@sismo-core/sismo-connect-server`**](../../technical-documentation/sismo-connect/server.md) package.

First, you will need to import it:

```bash
yarn add @sismo-core/sismo-connect-server
```

After importing, the first step is to create a `sismoConnectConfig` for your server and initialize a new `SismoConnect` server instance with it. We can do this in one line of code.

```typescript
// in your backend
import { 
  AuthType, 
  SismoConnect,
  SismoConnectVerifiedResult 
} from '@sismo-core/sismo-connect-server';

const sismoConnect = SismoConnect({appId: "0x0ad03e347304dd6c19d9aa75db8659d9"});
```

With this `SismoConnect` instance, you can call the `verify` function which takes the `sismoConnectResponse` sent by the frontend and returns a `SismoConnectVerifiedResult`.

<pre class="language-typescript"><code class="lang-typescript"><strong>const result: SismoConnectVerifiedResult = await sismoConnect.verify(sismoConnectResponse, {
</strong>    auths: [{authType: AuthType.VAULT}]
});

// you can get the anonymized userId with the helper `getUserId`
// and by specifying the authType as `VAULT`
const anonUserId = result.getUserId(AuthType.VAULT);
</code></pre>

If the proof is valid, an anonUserId is returned. If not, an error is received.

The `anonUserId` corresponds to a unique identifier for the user Data Vault, the best part of it is that this `anonUserId` is derived from the `appId` so it is impossible to compare two identifiers from two different apps implementing sismoConnect. **The `anonUserId` can be used in your application as a user identifier**.

A user has now the ability to privately authenticate himself in any application integrating sismoConnect by proving that he owns a Data Vault. 🤘

{% hint style="success" %}
You can view the complete backend integration for a simple auth [here](https://github.com/sismo-core/sismo-connect-boilerplate/blob/main/src/pages/api/verify-simple-auth.ts).
{% endhint %}

### Deploy your app (optional)

If you chose to use Next.js, we recommended using the Vercel service for deployment. With Vercel you will deploy your frontend and your backend in two clicks.&#x20;

Here's how to get started:

1. Create an account on [Vercel](https://vercel.com/) with your Github account
2. Create a new project and import your zksub repository in Vercel

<figure><img src="../../.gitbook/assets/Screenshot 2023-03-21 at 10.00.30 (1).png" alt=""><figcaption><p>Import a Git repository</p></figcaption></figure>

3. When your repository is linked to Vercel, you should see this page appear:

<figure><img src="../../.gitbook/assets/Screenshot 2023-03-21 at 10.38.32.png" alt=""><figcaption><p>Configure your project</p></figcaption></figure>

Click on "Deploy", and congratulations! Your app is now deployed at https://\[Name of your app].vercel.app. 💪

### **Next steps**

All Data can be categorized into groups. If you liked this tutorial, you can see how to leverage sismoConnect integration by asking for proof of user group membership in a privacy-preserving way with [this one](request-data-privately-with-sismo-connect.md).&#x20;

If you have any questions about integrating sismoConnect, don’t hesitate to reach out. The team will be happy to answer any questions you may have.

Get involved in the Sismo community:

* Look out for [**hackathons**](https://www.notion.so/sismo/Sismo-x-ETHPorto-2023-cbda827ea5f2469aa5fdbb4955fc18d6?pvs=4) that we are participating in
* Join our [**Discord**](https://discord.gg/sismo) or our [**Dev** **Telegram**](https://t.me/+Z-SwcvXZFRVhZTQ0)
* See the [**Sismo-hub contributing page**](https://github.com/sismo-core/sismo-hub/issues)
