# Sismo Connect Configuration

The `SismoConnectConfiguration` is the first object to understand when starting Sismo Connect integration in your application. Its main feature is to reference the `appId` you got from the **Sismo Factory** [when you created a new Sismo Connect app](../tutorials/create-a-sismo-connect-app.md). It will then enable you to request proofs from your users and verify them.

## Type definitions&#x20;

The `SismoConnectConfiguration` is an object with the following properties:

* `appId` (required): the Sismo Connect application identifier that you have created on the Sismo Factory.
* `vault.impersonate` (optional): by default set to `null`. The list of Data Sources that you want to impersonate. They will be automatically added to your developer Vault.
* `displayRawResponse` (optional): by default set to `false`. If set to `true`, the Sismo Data Vault app will display the proof generated in a modal and will not redirect to your application once the proof is generated. It can be useful in development.&#x20;
* `sismoApiUrl` (optional): API endpoint to fetch group information.
* `vaultAppBaseUrl` (optional): by default set to `https://vault-beta.sismo.io/`.

```typescript
type VaultConfig = {
    impersonate: string[];
};

type SismoConnectConfig = {
    appId: string;
    vault?: VaultConfig;
    displayRawResponse?: boolean;
    sismoApiUrl?: string;
    vaultAppBaseUrl?: string;
};
```

{% hint style="info" %}
As far as the configurations are the same in the client and server packages, **we highly recommend having a common config located in a separate file** (`sismo-connect-config.ts` for example) **if you have a mono repo**. You can easily import the configuration in your front end and back end from this file.
{% endhint %}

## Integration

Requests are made in the front end using either the [`sismo-connect-react` package](packages/react.md) or the [`sismo-connect-client` package](packages/client.md).

Requests are then verified either in a back end using the [`sismo-connect-server` package](packages/server.md) or in a smart contract using the [`sismo-connect-solidity` package](packages/solidity.md).

{% tabs %}
{% tab title="React.js" %}
In your React application, the most basic Sismo Connect Config looks like this:

```jsx
import { SismoConnectConfig, SismoConnectButton } from "@sismo-core/sismo-connect-react";

const config: SismoConnectConfig = {
  // you will need to get an appId from the Factory
  appId: "0xf4977993e52606cfd67b7a1cde717069", 
}

<SismoConnectButton
    // config
    config={config}

    // Sismo Connect Request Data
    // request proof of Data Sources ownership (e.g EVM, GitHub, Twitter or Telegram)
    auths={[{ authType: AuthType.GITHUB }]}
    // request proof of Data Group memberships of source
    // (e.g part of NFT owners, Dao Participants, GitHub committers)
    claims={[{groupId: ENS_DAO_VOTERS_GROUP_ID}]}
    // request message signature from users.
    signature={{message: "I vote Yes to Privacy"}}
    onResponseBytes={(response: string) => {
        // call your contract/backend with the response as bytes
    }
/>

```
{% endtab %}

{% tab title="Typescript (client)" %}
In your front-end application, the most basic Sismo Connect Config looks like this:

```typescript
import { SismoConnectConfig, SismoConnect } from "@sismo-core/sismo-connect-client";

const config: SismoConnectConfig = {
  // you will need to get an appId from the Factory
  appId: "0xf4977993e52606cfd67b7a1cde717069", 
}

const sismoConnect = SismoConnect({config});
```
{% endtab %}

{% tab title="Node.js" %}
In your node.js backend, the most basic Sismo Connect Config looks like this:

```typescript
import { SismoConnectConfig, SismoConnect } from "@sismo-core/sismo-connect-server";

const config: SismoConnectConfig = {
  // you will need to get an appId from the Factory
  appId: "0xf4977993e52606cfd67b7a1cde717069", 
}

const sismoConnect = SismoConnect({config});
```
{% endtab %}

{% tab title="Solidity (foundry)" %}
In your smart contract, the most basic Sismo Connect Config looks like this:

```solidity
import "sismo-connect-solidity/SismoConnectLib.sol"; // <--- add a Sismo Connect import

contract MyContract is SismoConnect {
  bytes16 private _appId = 0xf4977993e52606cfd67b7a1cde717069;
  
  constructor()
    SismoConnect(buildConfig(_appId)) // <--- Sismo Connect constructor
  {}
}
```
{% endtab %}
{% endtabs %}

## Impersonating accounts in development

Impersonating accounts allows you to be redirected to a Sismo Data Vault app with fake accounts imported that you defined in the `SismoConnectConfig` object. The impersonated Vault only exists in your browser memory. It does not affect your personal Data Vault.&#x20;

It is useful in development to test your application flow. You are able to generate a proof based on a Sismo Connect request for which you would not be eligible otherwise.&#x20;

{% hint style="warning" %}
Impersonation should not be used in production.
{% endhint %}

<figure><img src="../../.gitbook/assets/Capture d’écran 2023-06-16 à 13.17.15.png" alt=""><figcaption></figcaption></figure>

{% tabs %}
{% tab title="React.js" %}
In your React application, the impersonation Sismo Connect Config looks like this:

```jsx
import { SismoConnectConfig, SismoConnectButton } from "@sismo-core/sismo-connect-react";

const config: SismoConnectConfig = {
  // you will need to get an appId from the Factory
  appId: "0xf4977993e52606cfd67b7a1cde717069", 
  vault: {
    // For development purposes insert the identifier that you want to impersonate here
    // Never use this in production
    impersonate: [
      "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
      "github:leosayous21",
      "twitter:dhadrien_:2390703980",
    ]
  }
}

<SismoConnectButton
    config={config}
    
    // Sismo Connect Request
    // request proof of Data Sources ownership (e.g EVM, GitHub, Twitter or Telegram)
    auths={[{ authType: AuthType.GITHUB }]}
    // request proof of Data Group memberships of source
    // (e.g part of NFT owners, Dao Participants, GitHub committers)
    claims={[{groupId: ENS_DAO_VOTERS_GROUP_ID}]}
    // request message signature from users.
    signature={{message: "I vote Yes to Privacy"}}
    onResponseBytes={(response: string) => {
        // call your contract with the response as bytes
    }
/>

```
{% endtab %}

{% tab title="Typescript (client)" %}
In your front-end application, the impersonation Sismo Connect Config looks like this:

```typescript
import { SismoConnectConfig, SismoConnect } from "@sismo-core/sismo-connect-client";

const config: SismoConnectConfig = {
  // you will need to get an appId from the Factory
  appId: "0xf4977993e52606cfd67b7a1cde717069", 
  vault: {
    // For development purposes insert the identifier that you want to impersonate here
    // Never use this in production
    impersonate: [
      "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
      "github:leosayous21",
      "twitter:dhadrien_:2390703980",
    ]
  }
}

const sismoConnect = SismoConnect({config});
```
{% endtab %}

{% tab title="Node.js" %}
In your node.js backend, the impersonation Sismo Connect Config looks like this:

```typescript
import { SismoConnectConfig, SismoConnect } from "@sismo-core/sismo-connect-server";

const config: SismoConnectConfig = {
  // you will need to get an appId from the Factory
  appId: "0xf4977993e52606cfd67b7a1cde717069", 
  vault: {
    // For development purposes insert the identifier that you want to impersonate here
    // Never use this in production
    impersonate: [
      "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
      "github:leosayous21",
      "twitter:dhadrien_:2390703980",
    ]
  }
}

const sismoConnect = SismoConnect({config});
```
{% endtab %}

{% tab title="Solidity (foundry)" %}
In your smart contract, the most basic Sismo Connect Config looks like this:

```solidity
import "sismo-connect-solidity/SismoConnectLib.sol"; // <--- add a Sismo Connect import

contract MyContract is SismoConnect {
  bytes16 private _appId = 0xf4977993e52606cfd67b7a1cde717069;
  bool private _isImpersonationMode = true;
  
  constructor()
    SismoConnect(buildConfig(_appId, _isImpersonationMode))
  {}
}
```
{% endtab %}
{% endtabs %}
