# Request data privately with Sismo Connect

### What’s inside?

This tutorial will walk you through how to request data from your users privately thanks to [**Sismo Connect**](../../readme/sismo-connect.md) integration. This tutorial will not be centered around only one example but it is meant to showcase many ways of using Sismo Connect in your app.&#x20;

To get a feel of what you could build with this tutorial, you can try out a demo [**here**](https://demo.zksub.io/)**.** This Sismo Connect app enables  whitelisted wallets and GitHub accounts to register to a service with their email without revealing their eligible account.

{% hint style="success" %}
You can find all the code showcased in this tutorial in [this open-source boilerplate repository](https://github.com/sismo-core/sismo-connect-boilerplate) showcasing multiple Sismo Connect integrations in Next.

The repository use the [`@sismo-core/sismo-connect-react`](../../technical-documentation/sismo-connect/react.md)package to request the proof, if your project is not on react you can use the[`@sismo-core/sismo-connect-client`](../../technical-documentation/sismo-connect/client.md)package`.`
{% endhint %}

### Choose your stack (Next.js recommended)

To implement Sismo Connect, you'll need both a frontend and a backend. The frontend will request the proof, and the backend will verify it.

For this tutorial, we recommend using the Next.js stack, which is a full-stack React framework. Next.js also offers a deployment service called Vercel, which makes it easy to deploy your app in just two clicks.&#x20;

To create a new Next.js project, run the following command:

```
yarn create next-app --typescript
```

That's it! Your frontend will be located in `src/pages/index.tsx`, and your backend will be located in `src/pages/api`.

You can find the complete Next.js boilerplate repository setup with Sismo Connect [here](https://github.com/sismo-core/sismo-connect-boilerplate).

### Register your Sismo Connect App in the Factory

Before you begin integrating [**Sismo Connect**](../../readme/sismo-connect.md), you must create first a Sismo Connect app in the [**Sismo Factory**](https://factory.sismo.io/apps-explorer). This step is mandatory to obtain an application Id (`appId`), which is required during the Sismo Connect development process.

<details>

<summary>Why is an <code>appId</code> mandatory for Sismo Connect?</summary>

The `appId` will be used to compute an AnonUserID, which is the the unique identifier for a user on your app. The AnonUserID is simply the hash of a user's Vault secret and the appId.

$$vaultId = hash(vaultSecret, appId)$$

If we remove the appId from this simple calculation, we would have had the same AnonUserID for the same vaultSecret, effectively leaking information about a user that uses zkConnect on two different apps. The AnonUserID would be the same across different apps, and the user could be tracked if the AnonUserIDs became public.

By introducing an appId, the vaultId is now different between apps, and the same user will have two different AnonUserIDs on two different apps, effectively preserving the user's privacy.&#x20;

You can learn more about this notion in this [article](../../technical-concepts/vault-and-proof-identifiers.md).

</details>

<figure><img src="../../.gitbook/assets/Capture d’écran 2023-04-12 à 11.26.24.png" alt=""><figcaption><p>Register your Sismo Connect app in the Factory</p></figcaption></figure>

You can register a Sismo Connect app here: [https://factory.sismo.io/apps-explorer](https://factory.sismo.io/apps-explorer).\
\
To create a Sismo Connect app, you need to log in with Sign-In With Ethereum and click on “create a new Sismo Connect app”. You will need to register an App Name, enter a description, and upload a logo alongside registering authorized domains. Pay attention to authorized domains, as these are the urls where the appId that will be created can be used for [Sismo Connect](../../readme/sismo-connect.md).&#x20;

{% hint style="info" %}
Feel free to add `*.com` to authorized domains when following along this tutorial. This will allow to whitelist `localhost`.
{% endhint %}

Once created, you should have all information about your app displayed in your profile:

<figure><img src="../../.gitbook/assets/Capture d’écran 2023-04-12 à 16.04.56.png" alt=""><figcaption><p>Your Sismo Connect apps profile</p></figcaption></figure>

The appId displayed on your app’s profile in the Factory is its unique identifier. You will use it to request proof from your users in your app’s frontend and to verify it in the backend.

For this guide, you can use your own `appId`, such as `0x0ad03e347304dd6c19d9aa75db8659d9`.

### Select or create your group of users and get the groupId

The core concept of Sismo Connect is to enable users to selectively disclose statements to applications regarding pieces of personal data, that we call Data Shards, stored in their Data Vault. This is done through the use of zero-knowledge proofs. Data Shards originate from groups that can also be created in the Factory.

All groups created on the Sismo protocol are public, so if someone has already created a group that meets your needs, you can reuse it. You can explore all existing groups on our [**Factory explorer**](https://factory.sismo.io/groups-explorer). If you have very specific needs, you can create your own group using our [**no-code interface**](https://factory.sismo.io/groups-explorer) or by following this [**tutorial**](../sismo-hub/create-your-group.md) to submit a pull request to our repository.

If you create a new group in the Factory, when members of this group will add their account in their Data Vault, they will get the associated Data Shard and will be able to start proving statements from it (e.g. I own one shard of “contributors to The Merge” group) when using Sismo Connect.

To use a group in a Sismo Connect app, we need to find its unique group ID (groupId). To do so, you can use the [group explorer in the Factory](https://factory.sismo.io/groups-explorer?search=sismo-contributors):

<figure><img src="../../.gitbook/assets/Capture d’écran 2023-04-12 à 16.10.37.png" alt=""><figcaption><p>sismo-contributors search result in the group explorer</p></figcaption></figure>

It’s better to try it out yourself, but you should ultimately find the group’s unique id that interests you, we will take the groupId of `sismo-contributors` group for example `0xe9ed316946d3d98dfcd829a53ec9822e`

Congratulations! You now have an appId (`0x0ad03e347304dd6c19d9aa75db8659d9`) and a groupId (`0xe9ed316946d3d98dfcd829a53ec9822e`), which means you can request some data from your users with [Sismo Connect](../../readme/sismo-connect.md). Let’s focus on how to request and verify proofs.

### Request Data ownership proof from your users on your frontend

Now that you have an app and a group that users should prove membership in, you need to send your users to the Sismo Vault app to generate the required proof. When redirected, users will be able to privately create a proof from their Data Vault and the personal piece of data that comes from the requested group in Sismo Connect before sending it to you.

To do that you will need to use one of our packages:

* Javascript / Typescript: [`@sismo-core/sismo-connect-client`](../../technical-documentation/sismo-connect/client.md)
* React / Next: [`@sismo-core/sismo-connect-react`](../../technical-documentation/sismo-connect/react.md)

The following snippets will showcase the React usage but feel free to see the [`@sismo-core/sismo-connect-client`](../../technical-documentation/sismo-connect/client.md) documentation for more vanilla javascript/typescript integration.

First, you will need to import the React package:

```bash
yarn add @sismo-core/sismo-connect-react
```

After importing, you will be able to use the Sismo Connect button in your app. By clicking on this button your user will be redirected to the [Data Vault App](https://docs.sismo.io/sismo-docs/technical-documentation/data-vault-app), to generate a proof for the specified group.

<figure><img src="../../.gitbook/assets/Capture d’écran 2023-04-12 à 11.30.56.png" alt=""><figcaption><p>Sismo Connect button</p></figcaption></figure>

To do so, you have to use the `SismoConnectButton` component, you can see below several examples one how to use the button and request different proofs from your user.&#x20;

{% tabs %}
{% tab title="Claim / Vault Auth" %}
Here is the request we make in the frontend for the rest of the tutorial. You can see a more complex request on the other tab.

```typescript
import {
  SismoConnectButton,
  SismoConnectClientConfig,
  SismoConnectResponse,
  AuthType
} from "@sismo-core/sismo-connect-react";

export const config: SismoConnectClientConfig = {
  appId: "0x0ad03e347304dd6c19d9aa75db8659d9" // appId you registered
};

 <SismoConnectButton
    config={config}
    // request a proof of Data Vault Ownership
    auths={[{authType: AuthType.VAULT}]}
    // request a proof of group membership for `sismo-contributors`
    claims={[{ groupId: "0xe9ed316946d3d98dfcd829a53ec9822e" }]}
    // get a response
    onResponse={(response: SismoConnectResponse) => {
    //Send the response to your server to verify it
    //thanks to the @sismo-core/sismo-connect-server package
    //Will see how to do this in next part of this tutorial
  }}
/>
```

The `auths` field will request a proof of Data Vault ownership and the `claims` field will request a proof of group membership for the `sismo-contributors` group.

{% hint style="info" %}
The `dataRequest` has more optional parameters available:

* **`groupTimestamp`**: This parameter specifies the timestamp of the group snapshot a user wants to prove membership in. By default, this timestamp is set to ‘latest’ to use the latest generated snapshot.
* **`value`**: In groups, every account is associated with a value, which can represent the number of tokens staked, the voting power of the account, etc. The `requestedValue` parameter allows you to specify the value your user needs to have in the group to generate the proof. By default, it is set to 1.
* **`claimType`**: The claimType can force users to prove that they have a value in the group greater than or equal (`ClaimType.GTE`) to the `requested value` or strictly equal (`ClaimType.EQ`)to the `requested value`. By default, it is `“GTE”.`\


You can see the [documentation](../../technical-documentation/sismo-connect/client.md) if you want to learn more about this.&#x20;
{% endhint %}
{% endtab %}

{% tab title="Claim / Two Auths / Signature" %}
You can request optional proofs. It is up to the user to choose to share his data with your app or not. You can also make your users sign a message.

```typescript
import {
  SismoConnectButton,
  SismoConnectClientConfig,
  SismoConnectResponse,
  AuthType
} from "@sismo-core/sismo-connect-react";

export const config: SismoConnectClientConfig = {
  appId: "0x0ad03e347304dd6c19d9aa75db8659d9" // appId you registered
};

 <SismoConnectButton
    config={config}
    auths={[
       // request a proof of Twitter account Ownership (optional)
       {authType: AuthType.TWITTER, isOptional: true},
       // request a proof of GitHub account Ownership (required)
       {authType: AuthType.GITHUB},
    ]}
    // request a proof of group membership for `sismo-contributors`
    claims={[{ groupId: "0xe9ed316946d3d98dfcd829a53ec9822e" }]}
    // make your users sign a message that makes sense for your app
    signature={{message: "0x1234568"}}
    // get a response
    onResponse={(response: SismoConnectResponse) => {
    //Send the response to your server to verify it
    //thanks to the @sismo-core/zk-connect-server package
    //Will see how to do this in next part of this tutorial
  }}
/>
```

The `auths` field will request a proof of Twitter account ownership (that is optional) and a proof of GitHub account ownership (that is required), the `claims` field will request a proof of group membership for the `sismo-contributors` group and the `signature` field will introduce the `message` in every proofs.
{% endtab %}
{% endtabs %}

{% hint style="info" %}
If you are not part of the group, feel free to use the optional `devMode` in your sismoConnectConfig. Set the boolean field enabled as true and input a list of groups that will be used instead of the actual Sismo groups. Beware that this devMode should only be used when prototyping, if you keep such config in production, these groups will be able to make proofs in their Vaults.
{% endhint %}

```typescript
import {
  SismoConnectButton,
  SismoConnectClientConfig,
  SismoConnectResponse,
  AuthType
} from "@sismo-core/sismo-connect-react";

const config: SismoConnectClientConfig = {
    appId: "0x0ad03e347304dd6c19d9aa75db8659d9", // appId you registered
    devMode: {
      // will use the Dev Sismo Data Vault https://dev.vault-beta.sismo.io/
      enabled: true, 
      // overrides a group with these addresses
      devGroups: [{
        groupId: "0xe9ed316946d3d98dfcd829a53ec9822e",
        data: {
          "0x123...abc": 1, 
          "0x456...efa": 2
        },
      }]
    }
};

// you can use the button as before
```

After your user generates the proof, he will be automatically redirected back to your app. The `onResponse` props will allow you to get the response containing the proof.

{% hint style="success" %}
You can find all the frontend code snippets in the [boilerplate repository](https://github.com/sismo-core/sismo-connect-boilerplate).
{% endhint %}

Well done! You’re halfway there! 💪

Now that you have the proof, you need to verify the proof in your backend to be sure that the proof is valid with respect to your group (`sismo-contributors` in the example). If yes, you will allow your user to register his email.

### Verify the Data Proof in your backend (or soon in your smart contract)

A general rule in web development is that frontend input cannot be trusted.

Once you have obtained the proof from the frontend, you should send it to the backend for verification.

In your back end, you need to check the following:

* The validity of the cryptographic proof
* If the proof corresponds to proof of membership in the requested group

If these two conditions are checked, you know that the proof was generated by a Sismo Contributor without knowing their identity. 🤯

To do that, you will need to use the [**`@sismo-core/sismo-connect-server`**](../../technical-documentation/sismo-connect/server.md) package.

First, you will need to import it:

```bash
yarn add @sismo-core/sismo-connect-server
```

After importing, the first step is to create a `sismoConnectConfig` for your server and initialize a new SismoConnect server instance with it:

```typescript
import { 
  SismoConnect, SismoConnectServerConfig, AuthType, SismoConnectVerifiedResult 
} from '@sismo-core/sismo-connect-server';

const sismoConnectConfig: SismoConnectServerConfig = {
  appId: "0x0ad03e347304dd6c19d9aa75db8659d9", // appId you registered
}

const sismoConnect = SismoConnect(sismoConnectConfig);
```

{% hint style="info" %}
You can enable the devMode in the backend to only verify cryptographically the proof without checking the group.

In conclusion, the devMode allows to generate a cryptographically valid proof for whatever address added in the client config while checking that the proof is only cryptographically valid in the backend.

Always remember to have the devMode enabled in the frontend and the backend if you want to use it when prototyping.
{% endhint %}

```typescript
import { 
  SismoConnect, SismoConnectServerConfig, AuthType, SismoConnectVerifiedResult 
} from '@sismo-core/sismo-connect-server';

const sismoConnectConfig: SismoConnectServerConfig = {
  appId: "0x0ad03e347304dd6c19d9aa75db8659d9", // appId you registered
  devMode: {
    enabled: true
  }
}

const sismoConnect = SismoConnect(sismoConnectConfig);
```

With this SismoConnect instance, you can call the `verify` function which takes the `claims` , the `auths` and the `sismoConnectResponse` sent by the frontend:

```typescript

const result: SismoConnectVerifiedResult = await Sismo Connect.verify(response, {
    // the group memebership we want to check
    claims: [{groupId: "0xe9ed316946d3d98dfcd829a53ec9822e"}],
    // the Data Vault ownership we want to check
    auths: [{authType: AuthType.VAULT}],
 });

// we get an anonymized userId when we request a proof of Data Vault ownership
const anonUserId = result.getUserId(AuthType.VAULT));
```

{% hint style="info" %}
By doing this, you'll be able to verify the validity of the proof for the requested group. The `claims` field is crucial here as far as users could gain unauthorized access by providing valid proof for any group.&#x20;

We also check that the proof is a Data Vault proof of ownership.
{% endhint %}

If the proof and the requested group are valid, an anonUserId is returned. If not, an error is received.

The anonUserId corresponds to a unique identifier for the user Data Vault, the best part of it is that this anonUserId is derived from the appId so it is impossible to compare two anon user ids from two different apps implementing Sismo Connect.

A user has now the ability to authenticate himself by sharing only proof of private data he owns while not leaking any addresses or accounts where this data comes from. 🤘

{% hint style="success" %}
You can find the full code snippet for the backend [here](https://github.com/sismo-core/sismo-connect-boilerplate/blob/main/src/pages/api/verify-auth-and-claim.ts).
{% endhint %}

With the anon userId, you can now start to have a database without knowing which sismo contributor has used your app. You can for example allow them to register an email address for a waiting list but you still don't know their identity 🤯 You are now at 95% of the job! There is one last issue to resolve: what if one of your users adds multiple emails to gain access more than once to the waiting list?

### Avoid double spending with anonUserId

As you saw, the verify function will return you the anonUserId.

This anonUserId can be used as a user identifier, it is deterministically generated based on the following elements:

* The vault used to generate the proof
* The app ID

This means that the anonUserId will remain constant for a specific app if your user tries to prove something twice.

With this property, we can check if a user has already generated a proof for your app.

To do so, we only need to store the anonUserId with an added email for example in a context of an anonymized wiating list, and every time a proof is sent to the back end, check that the anonUserId returned is not already associated with an email address.

And that’s it! 💜

You are now able to request proofs from your users and verify them off chain without breaching their privacy, thanks to zero-knowledge proofs.

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

As previously mentioned, all Data can be categorized into [Groups](../../technical-concepts/data-groups.md). This means you are now able to create apps where users can prove anything about their identities in a privacy-preserving manner. Get creative, as the possibilities are endless!

If you have any questions about integrating Sismo Connect, don’t hesitate to reach out. The team will be happy to answer any questions you may have.

Get involved in the Sismo community:

* Look out for [**hackathons**](https://www.notion.so/sismo/Sismo-x-ETHPorto-2023-cbda827ea5f2469aa5fdbb4955fc18d6?pvs=4) that we are participating in
* Join our [**Discord**](https://discord.gg/sismo) or our [**Dev Telegram**](https://t.me/+Z-SwcvXZFRVhZTQ0)
* See the [**Sismo-hub contributing page**](https://github.com/sismo-core/sismo-hub/issues)
