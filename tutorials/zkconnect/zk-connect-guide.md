# Request data privately with zkConnect

### What’s inside?

This tutorial will walk you through how we built zkSub, an application enabling whitelisted wallets and GitHub accounts to register to a service with their email without revealing their eligible account thanks to [**zkConnect**](../../what-is-sismo/zkconnect.md) integration.

To get a feel of what you will build in this tutorial, you can try out a demo [**here**](https://demo.zksub.io/).

{% hint style="info" %}
You can access the open-source repository of the demo:

* In React [here](https://github.com/sismo-core/zksub)
* In Next.js [here](https://github.com/sismo-core/zksub-next)

These 2 repositories use the [`@sismo-core/zk-connect-react`](https://docs.sismo.io/sismo-docs/technical-documentation/zkconnect/zkconnect-react-request)`package to request the proof, if your project is not on react you can use the`[`@sismo-core/zk-connect-client`](../../technical-documentation/zkconnect/zkconnect-client-request.md)`package.`
{% endhint %}

### Tutorial use case

We will use [**The Ethereum Community Conference**](https://www.ethcc.io/) (EthCC), the largest annual European Ethereum event focused on technology and community, as an example in this tutorial.

Indeed, EthCC wants to grant premium access to contributors to [**The Merge**](https://ethereum.org/en/upgrades/merge/). As a number of Ethereum contributors are anonymous, they may want to avoid revealing their personal information to EthCC organizers while still proving that they contributed to The Merge and deserve a premium access.

For anonymous contributors to The Merge to attend EthCC, we will build zkSub that enables them to register an email address and subsequently prove they are contributors to The Merge in a privacy-preserving manner. These anonymous contributors will then receive premium access to EthCC via email.

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

Before you begin integrating [**zkConnect**](../../what-is-sismo/zkconnect.md), you must create first a zkConnect app in the [**Sismo Factory**](https://factory.sismo.io/apps-explorer). This step is mandatory to obtain an application Id (`appId`), which is required during the zkConnect development process.

<details>

<summary>Why is an <code>appId</code> mandatory for zkConnect?</summary>

The `appId` will be used to compute the vault identifier which is the the unique identifier for a user creating a proof for your app. The vault identifier is simply the hash of the user vault secret and the appId.

$$vaultId = hash(vaultSecret, appId)$$

If we had removed the appId from this simple calculus, we would have had the same vaultIdentifier for the same vaultSecret, effectively leaking information about a user that uses zkConnect on two different apps. The vaultId would just be the same between the apps and the user can be tracked if vault identifiers become public.

But if we introduce an appId, the vaultId is now different between apps and the same user will have two different vault ids between two different apps, effectively preserving the user privacy this time. 🤘&#x20;

You can learn more about this notion in the [Vault & Proof identifiers article](../../technical-concepts/vault-and-proof-identifiers.md).

</details>

<figure><img src="../../.gitbook/assets/Capture d’écran 2023-03-14 à 19.47.52 (1).png" alt=""><figcaption><p>Register your zkConnect app in the Factory</p></figcaption></figure>

You can register a zkConnect app here: [https://factory.sismo.io/apps-explorer](https://factory.sismo.io/apps-explorer).\
\
To create a zkConnect App, you need to login with Sign-In With Ethereum and click on “create a new zkConnect App”. You will need to register an App Name, a description and upload a logo alongside registering authorized domains. You will need to pay attention about authorized domains as far as these are all the urls from which the appId that will be created can be used for zkConnect. Here we only reference [ethcc.io](http://ethcc.io/) for example.

Once created, you should have all information about your app displayed in your profile:

<figure><img src="../../.gitbook/assets/Capture d’écran 2023-03-14 à 19.56.02.png" alt=""><figcaption><p>Your zkConnect apps profile</p></figcaption></figure>

The appId displayed on your app’s profile in the Factory is its unique identifier. You will use it to request proof from your users in your app’s frontend and to verify it in the backend.

For this guide, you can use your own `appId`, such as `0x8f347ca31790557391cec39b06f02dc2`.

### Select or create your group of users and get the groupId

The core concept of ZK Connect is to enable users to selectively disclose statements to applications regarding pieces of personal data, that we call Data Shards, stored in their Data Vault. This is done through the use of zero-knowledge proofs. Data Shards originate from groups that can also be created in the Factory.

All groups created on the Sismo protocol are public, so if someone has already created a group that meets your needs, you can reuse it. You can explore all existing groups on our [**Factory explorer**](https://factory.sismo.io/groups-explorer). If you have very specific needs, you can create your own group using our [**no-code interface**](https://factory.sismo.io/groups-explorer) or by following this [**tutorial**](../sismo-hub/create-your-group.md) to submit a pull request to our repository.

For the EthCC app, we will use the group with all users who contributed to The Merge named [`the-merge-contributor`](https://github.com/sismo-core/sismo-hub/blob/main/group-generators/generators/the-merge-contributor/index.ts). When member of this group will add their account in their Data Vault, they will get the associated Data Shard and will be able to start proving statements from it (e.g. I own one shard of “contributors to The Merge” group) when using zkConnect.

Now that we've decided to use this group in our app, we need to find its unique group ID (groupId) to use it in the zkConnect flow.

To do so, you can use the [group explorer in the Factory](https://factory.sismo.io/groups-explorer?search=the-mer):

<figure><img src="../../.gitbook/assets/Capture d’écran 2023-03-14 à 13.54.49.png" alt=""><figcaption><p>the-merge-contributor search result in the group explorer</p></figcaption></figure>

It’s better to try it out yourself, but you should ultimately find the group’s unique id: `0x42c768bb8ae79e4c5c05d3b51a4ec74a`

Congratulations! You now have an appId (`0x8f347ca31790557391cec39b06f02dc2`) and a groupId (`0x42c768bb8ae79e4c5c05d3b51a4ec74a`), which means you can integrate [zkConnect](../../what-is-sismo/zkconnect.md) into your app. Let’s now focus on how to generate and verify the proof.

### Request Data ownership proof from your users on your frontend

Now that you have an app and the group that users should prove membership in, you need to send your users to the Sismo Vault app to generate the required proof. When redirected, users will be able to privately create a proof from their Data Vault and the personal piece of data that comes from the requested group in zkConnect before sending it to you.

To do that you will need to use one of our packages:

* Javascript / Typescript: [`@sismo-core/zk-connect-client`](../../technical-documentation/zkconnect/zkconnect-client-request.md)
* React: [`@sismo-core/zk-connect-react`](https://docs.sismo.io/sismo-docs/technical-documentation/zkconnect/zkconnect-react-request)``

{% tabs %}
{% tab title="React / Next.js" %}
First, you will need to import the following:

```bash
yarn add @sismo-core/zk-connect-react
```

\
After importing, you will be able to use the zkConnect button in your app.

<figure><img src="../../.gitbook/assets/zkConnect (1).png" alt=""><figcaption><p>zkConnect button</p></figcaption></figure>



To do so, you have to use the `ZkConnectButton` component:

```typescript
import { ZkConnectButton, ZkConnectResponse, AuthType } from "@sismo-core/zk-connect-react";

<ZkConnectButton 
  appId={"0x8f347ca31790557391cec39b06f02dc2"} // appId you registered
  // Declare your claimRequest for the-merge-contributor group
  claimRequest={{
    groupId: "0x42c768bb8ae79e4c5c05d3b51a4ec74a" 
  }},
  authRequest: {{
    authType: AuthType.ANON
  }}
  // get a response 
  onResponse={async (zkConnectResponse: ZkConnectResponse) => {
    //Send the response to your server to verify it
    //thanks to the @sismo-core/zk-connect-server package
    //Will see how to do this in next part of this tutorial
  }}
/>
```



With the `claimRequest` props, you can request your users to generate a proof for a requested groupId, here the group `the-merge-contributor`. With the authRequest with authType AuthType.ANON you can request an anonUserId.



{% hint style="info" %}
The `dataRequest` has more optional parameters available:

* **`groupTimestamp`**: This parameter specifies the timestamp of the group snapshot a user wants to prove membership in. By default, this timestamp is set to ‘latest’ to use the latest generated snapshot.
* **`value`**: In groups, every account is associated with a value, which can represent the number of tokens staked, the voting power of the account, etc. The `requestedValue` parameter allows you to specify the value your user needs to have in the group to generate the proof. By default, it is set to 1.
* **`claimType`**: The claimType can force users to prove that they have a value in the group greater than or equal (`ClaimType.GTE`) to the `requested value` or strictly equal (`ClaimType.EQ`)to the `requested value`. By default, it is `“GTE”.`\
  ``

You can see the [documentation](../../technical-documentation/zkconnect/zkconnect-client-request.md) if you want to learn more about this.
{% endhint %}



By clicking on this button your user will be redirected to the [Data Vault App](https://docs.sismo.io/sismo-docs/technical-documentation/data-vault-app), to generate a proof for the specified group.&#x20;



{% hint style="info" %}
If you are not part of the group, feel free to use the optional `devMode` in your zkConnectConfig. Set the boolean field enabled as true and input a list of groups that will be used instead of the actual Sismo groups. Beware that this devMode should only be used when prototyping, if you keep such config in production, these groups will be able to make proofs in their Vaults.
{% endhint %}



```typescript
import { AuthType, ZkConnectButton, ZkConnectClientConfig, ZkConnectResponse } from "@sismo-core/zk-connect-react";

const config: ZkConnectClientConfig = {
    appId: "0x8f347ca31790557391cec39b06f02dc2", // ethcc appId I created
    devMode: {
      // will use the Dev Sismo Data Vault https://dev.vault-beta.sismo.io/
      enabled: true, 
      // overrides a group with these addresses
      devGroups: [{
        groupId: "0x42c768bb8ae79e4c5c05d3b51a4ec74a",
        data: {
          "0x123...abc": 1, 
          "0x456...efa": 2
        },
      }]
    }
};

<ZkConnectButton 
  config={config}
  // Declare your claimRequest for the-merge-contributor group
  claimRequest={{
    groupId: "0x42c768bb8ae79e4c5c05d3b51a4ec74a" 
  }}
  authRequest={{
    authType: AuthType.ANON
  }}
  // get a response 
  onResponse={async (zkConnectResponse: ZkConnectResponse) => {
    //Send the response to your server to verify it
    //thanks to the @sismo-core/zk-connect-server package
    //Will see how to do this in next part of this tutorial
  }}
/>
```



After your user generates the proof, he will be automatically redirected back to your app.

The `onResponse` props will allow you to get the response containing the proof.



You can find the full frontend code snippet used for zkSub:

* In React [here](https://github.com/sismo-core/zksub/blob/main/front/src/App.tsx)
* In Next.js [here](https://github.com/sismo-core/zksub-next/blob/main/src/pages/index.tsx)
{% endtab %}

{% tab title="Javascript / Typescript" %}
First, you will need to import the following:First, you will need to import the following:

```bash
yarn add @sismo-core/zk-connect-client
```



After importing, the first step is to create a `zkConnectConfig` for your client and initialize a new ZKConnect client instance with it:

<pre class="language-typescript"><code class="lang-typescript"><strong>import { ZkConnect, ZkConnectClientConfig } from "@sismo-core/zk-connect-client";
</strong>
const zkConnectConfig: ZkConnectClientConfig = {
  appId: "0x8f347ca31790557391cec39b06f02dc2", // ethcc appId I created
}
const zkConnect = new ZkConnect(zkConnectConfig);
</code></pre>



{% hint style="info" %}
If you are not part of the group, feel free to use the optional `devMode` in your zkConnectConfig. Set the boolean field enabled as true and input a list of groups that will be used instead of the actual Sismo groups. Beware that this devMode should only be used when prototyping, if you keep such config in production, these groups will be able to make proofs in their Vaults.
{% endhint %}



```typescript
import { ZkConnect, ZkConnectClientConfig } from "@sismo-core/zk-connect-client";

const zkConnectConfig: ZkConnectClientConfig = {
  appId: "0x8f347ca31790557391cec39b06f02dc2", // ethcc appId I created
  devMode?: {
    // will use the Dev Sismo Data Vault https://dev.vault-beta.sismo.io/
    enabled?: true, 
    // overrides a group with these addresses
    devGroups?: {
      groupId: "0x42c768bb8ae79e4c5c05d3b51a4ec74a",
      data: {
        "0x123...abc": 1, 
        "0x456...efa": 2
      },
  }
}

const zkConnect = new ZkConnect(zkConnectConfig);
```



With this ZkConnect instance, you can request your users to generate a proof for a requested groupId, here the group `the-merge-contributor`.

```typescript
// The `request` function sends your user to the Sismo vault app to generate the proof.
zkConnect.request({
  // we request a proof for a specific group
  claimRequest: { 
    groupId: "0x42c768bb8ae79e4c5c05d3b51a4ec74a" 
  },
  authRequest: {
    authType: AuthType.ANON
  }
});

// get a response 
const zkConnectResponse = zkConnect.getResponse();
```

By calling the `request` function, your users will be redirected to the Sismo Vault app to generate a proof for the specified group. The `getResponse` function will allow the user to send the proof to your frontend by being redirected when the proof generation is done in their Data Vault.

{% hint style="info" %}
The `dataRequest` has more optional parameters available:

* **`groupTimestamp`**: This parameter specifies the timestamp of the group snapshot a user wants to prove membership in. By default, this timestamp is set to ‘latest’ to use the latest generated snapshot.
* **`value`**: In groups, every account is associated with a value, which can represent the number of tokens staked, the voting power of the account, etc. The `requestedValue` parameter allows you to specify the value your user needs to have in the group to generate the proof. By default, it is set to 1.
* **claimType**: The claimType can force users to prove that they have a value in the group greater than or equal (`ClaimType.GTE`) to the `requested value` or strictly equal (`ClaimType.EQ`)to the `requested value`. By default, it is `“GTE”.`\
  ``

You can see the [documentation](../../technical-documentation/zkconnect/zkconnect-client-request.md) if you want to learn more about this.
{% endhint %}

After your user is redirected to your app, you need to receive the generated proof via `getResponse` .

```typescript
const zkConnectResponse = zkConnect.getResponse();
```

This will return you a `ZkConnectResponse` which contains the user proof which certifies the user is a member of the requested group.\


You can find the full frontend code snippet used for zkSub [here](https://github.com/sismo-core/zksub/blob/zk-connect-client-package/front/src/App.tsx).
{% endtab %}
{% endtabs %}

Well done! You’re halfway there! 💪

Now that you have the proof, you need to verify the proof in your backend to be sure that the proof is valid with respect to `the-merge-contributor` group. If yes, you will allow your user to register his email.



### Verify the Data Proof in your backend (or soon in your smart contract)

A general rule in web development is that frontend input cannot be trusted.

Once you have obtained the proof from the frontend, you should send it to the backend for verification.

In your back end, you need to check the following:

* The validity of the cryptographic proof
* If the proof corresponds to proof of membership in the requested group

If these two conditions are checked, you know that the proof was generated by a contributor to The Merge without knowing their identity. 🤯

To do that, you will need to use the [**`@sismo-core/zk-connect-server`**](../../technical-documentation/zkconnect/zkconnect-server-verify-off-chain.md) package.

First, you will need to import it:

```bash
yarn add @sismo-core/zk-connect-server
```

After importing, the first step is to create a `zkConnectConfig` for your server and initialize a new ZKConnect server instance with it:

```typescript
import { ZkConnect, ZkConnectServerConfig, DataRequest } from "@sismo-core/zk-connect-server";

const zkConnectConfig: ZkConnectServerConfig = {
  appId: "0x8f347ca31790557391cec39b06f02dc2",
}
const zkConnect = ZkConnect(zkConnectConfig);
```

{% hint style="info" %}
You can enable the devMode in the backend to only verify cryptographically the proof without checking the group.

In conclusion, the devMode allows to generate a cryptographically valid proof for whatever address added in the client config while checking that the proof is only cryptographically valid in the backend.

Always remember to have the devMode enabled in the frontend and the backend if you want to use it when prototyping.
{% endhint %}

```typescript
import { ZkConnect, ZkConnectServerConfig, DataRequest } from "@sismo-core/zk-connect-server";

const zkConnectConfig: ZkConnectServerConfig = {
  appId: "0x8f347ca31790557391cec39b06f02dc2",
  devMode: {
    enabled: true,
  }
}
const zkConnect = ZkConnect(zkConnectConfig);
```

With this ZkConnect instance, you can call the verify function which takes the claimRequest for the group and the `zkConnectResponse` sent by the frontend:

```typescript
const { verifiedAuths } = await zkConnect.verify(
  zkConnectResponse,
  { 
    claimRequest: { groupId: "0x42c768bb8ae79e4c5c05d3b51a4ec74a" }, 
    authRequest: { authType: AuthType.ANON}
  }
);

const anonUserId = verifiedAuths[0].userId;
```

{% hint style="info" %}
By doing this, you'll be able to verify the validity of the proof for the requested group. The ClaimRequest is crucial here as far as users could gain unauthorized access by providing valid proof for any group.
{% endhint %}

If the proof and the requested group are valid, an anonUserId is returned. If not, an error is received.

The anonUserId corresponds to a unique identifier for the user Data Vault, the best part of it is that this anonUserId is derived from the appId so it is impossible to compare two anon user ids from two different apps implementing zkConnect.

A user has now the ability to authenticate himself by sharing only proof of private data he owns while not leaking any addresses or accounts where this data comes from. 🤘

You can find the full code snippet for the backend [here](https://github.com/sismo-core/zksub/blob/main/back/src/index.ts).

You can now certify that the email sent to your API belongs to a contributor of The Merge without knowing which contributor. 🤯 You are now at 95% of the job! There is one last issue to resolve: what if one of your users adds multiple emails to gain access more than once?

### Avoid double spending with anonUserId

As you saw, the verify function will return you the anonUserId.

This anonUserId can be used as a user identifier, it is deterministically generated based on the following elements:

* The vault used to generate the proof
* The app ID

This means that the anonUserId will remain constant for a specific app if your user tries to prove something twice.

With this property, we can check if a user has already generated a proof to gain EthCC access.

To do so, we only need to store the anonUserId with the added email, and every time a proof is sent to the back end, check that the anonUserId returned is not already associated with an email address.

And that’s it! 💜

You are now able to obtain the email addresses of all contributors to The Merge and invite them to EthCC without breaching their privacy, thanks to zero-knowledge proofs.

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

As previously mentioned, all Data can be categorized into groups. This means you are now able to create apps where users can prove anything about their identities in a privacy-preserving manner. Get creative, as the possibilities are endless!

If you have any questions about integrating zkConnect, don’t hesitate to reach out. The team will be happy to answer any questions you may have.

Get involved in the Sismo community:

* Look out for [**hackathons**](https://www.notion.so/sismo/Sismo-x-ETHPorto-2023-cbda827ea5f2469aa5fdbb4955fc18d6?pvs=4) that we are participating in
* Join our [**Discord**](https://discord.gg/sismo) **** or our **** [**Dev Telegram**](https://t.me/+Z-SwcvXZFRVhZTQ0)****
* See the [**Sismo-hub contributing page**](https://github.com/sismo-core/sismo-hub/issues)****
