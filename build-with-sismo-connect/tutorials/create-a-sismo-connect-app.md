# Get your appId - Create a Sismo Connect App

Before you begin integrating [**Sismo Connect**](../../#sismo-connect-the-crypto-native-sso), you must register first a Sismo Connect app in the [**Sismo Factory**](https://factory.sismo.io/apps-explorer). This step is mandatory to obtain an application Id (`appId`), which is required during the Sismo Connect development process.

<details>

<summary>Why is an <code>appId</code> required for Sismo Connect?</summary>

The `appId` will be used to compute a VaultId, which is the the unique identifier for a user on your app. The VaultId is simply the hash of a user's Vault secret and the appId.

$$vaultId = hash(vaultSecret, appId)$$

If we remove the `appId` from this simple calculation, we would have had the same VaultId for the same vaultSecret, effectively leaking information about a user that uses Sismo Connect on two different apps. The VaultId would be the same across different apps, and the user could be tracked if the VaultIds became public.

By introducing an `appId`, the vaultId is now different between apps, and the same user will have two different VaultIds on two different apps, effectively preserving the user's privacy.

You can learn more about this notion in this [article](../technical-documentation/vault-and-proof-identifiers.md).

</details>

<figure><img src="../../.gitbook/assets/Capture d’écran 2023-05-10 à 09.59.04.png" alt=""><figcaption><p>Register your Sismo Connect App in the <a href="https://factory.sismo.io/apps-explorer">Sismo Factory</a></p></figcaption></figure>

You can register a Sismo Connect app here: [https://factory.sismo.io/apps-explorer](https://factory.sismo.io/apps-explorer).\
\
To create a Sismo Connect app, you need to log in with Sign-In With Ethereum and click on “create a new Sismo Connect app”. You will need to register an App Name, enter a description, and upload a logo alongside registering authorized domains. Pay attention to authorized domains, as these are the urls where the `appId` that will be created can be used for [Sismo Connect](../../#sismo-connect-the-crypto-native-sso).

{% hint style="info" %}
Feel free to add `*.com` to authorized domains when developing in local. This will allow you to easily whitelist `localhost`. Don't forget to update the authorized domains when deploying in production though.
{% endhint %}

Once created, you should have all information about your app displayed in your profile:

<figure><img src="../../.gitbook/assets/Capture d’écran 2023-05-10 à 09.55.48.png" alt=""><figcaption><p>my Sismo Connect App</p></figcaption></figure>

The `appId` displayed on your app’s profile in the Factory is its unique identifier. You will use it to request proof from your users in your app’s front end and to verify it in the back end or your smart contracts.

In this example, my `appId` is `0xde1d47d45d449d7efe9242e66a05e1c1`.
