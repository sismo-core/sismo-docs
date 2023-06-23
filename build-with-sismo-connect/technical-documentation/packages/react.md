---
description: Request proofs from your user on your React project
---

# Sismo Connect React: Request

The [Sismo Connect](../../../welcome-to-sismo/what-is-sismo-connect.md) React package is a wrapper of the Sismo Connect client package which is a package built on top of the [Sismo Data Vault](../../../knowledge-base/resources/technical-concepts/data-gems-and-data-groups.md) app (the prover) to easily request proofs from your users. It is strongly advised to read about the [**Sismo Connect client package**](client.md) to understand the React package.

### Installation

Install the Sismo Connect React package in your front end with `npm` or `yarn`:

```bash
# with npm
npm install @sismo-core/sismo-connect-react
# with yarn
yarn add @sismo-core/sismo-connect-react
```

{% hint style="success" %}
Make sure to have at least v18.15.0 as Node version. You can encounter issues with ethers dependencies if not.
{% endhint %}

## Usage

### Configuration

The first step for integrating Sismo Connect in your front end is to create a `SismoConnectClientConfig` like in the client package. This config will require an `appId` and can be customized with [optional fields.](react.md#sismoconnectclientconfig) You can go to the [Sismo Factory](../../tutorials/create-a-sismo-connect-app.md) to create a Sismo Connect App and get an `appId` ([here is a tutorial](../../tutorials/create-a-sismo-connect-app.md)).

```typescript
import { SismoConnect, SismoConnectConfig } from "@sismo-core/sismo-connect-react";

const config: SismoConnectConfig = {
  // you will need to get an appId from the Factory
  appId: "0xf4977993e52606cfd67b7a1cde717069", 
}
```

You can learn more about the Sismo Connect Client config in the [client package](client.md).

### Sismo Connect button

To easily integrate Sismo Connect into your front end, you can use the Sismo Connect button. Clicking on this button will redirect your users to the Sismo Data Vault app, where they can generate their proofs.

After users generate their proofs, they will be automatically redirected back to your app with a Sismo Connect Response containing the generated proofs.

<figure><img src="../../../.gitbook/assets/sign in with Sismo Buttonx1,5.png" alt=""><figcaption><p>Sismo Connect button</p></figcaption></figure>

To integrate it you have access through the package to the `SismoConnectButton` component.

```typescript
import { SismoConnectButton, AuthType,  SismoConnectConfig, SismoConnectResponse } from "@sismo-core/sismo-connect-react";

const config: SismoConnectConfig = {
  appId: "0xf4977993e52606cfd67b7a1cde717069", 
}

<SismoConnectButton 
    // the client config created
    config={config}
    // request a proof of account ownership 
    // (here Vault ownership)
    auth={{authType: AuthType.VAULT}}
    // request a proof of group membership 
    // (here the group with id 0x42c768bb8ae79e4c5c05d3b51a4ec74a)
    claim={{groupId: "0x42c768bb8ae79e4c5c05d3b51a4ec74a"}}
    // request a message signature
    signature={{message: "Your message"}}
    //  a response containing his proofs 
    onResponse={async (response: SismoConnectResponse) => {
	//Send the response to your server to verify it
	//thanks to the @sismo-core/sismo-connect-server package
    }}
    onResponseBytes={async (bytes: string) => {
        //Send the response to your contract to verify it
        //thanks to the @sismo-core/sismo-connect-solidity package
    }}
/>

// You can also create several auth and claim requests 
// in the same button

<SismoConnectButton 
    config={config}
    // request multiple proofs of account ownership 
    // (here Vault ownership and Twitter account ownership)
    auths={[
        {authType: AuthType.VAULT},
        {authType: AuthType.TWITTER},
    ]}
    // request multiple proofs of group membership 
    // (here the groups with id 0x42c768bb8ae79e4c5c05d3b51a4ec74a and 0x8b64c959a715c6b10aa8372100071ca7)
    claims={[
        {groupId: "0x42c768bb8ae79e4c5c05d3b51a4ec74a"},
        {groupId: "0x8b64c959a715c6b10aa8372100071ca7"}
    ]}
    signature={{message: "Your message"}}
    onResponse={async (response: SismoConnectResponse) => {
	//Send the response to your server to verify it
	//thanks to the @sismo-core/sismo-connect-server package
    }}
    onResponseBytes={async (bytes: string) => {
        //Send the response to your contract to verify it
        //thanks to the @sismo-core/sismo-connect-solidity package
    }}
/>
```

After your users generate the proofs, they will be automatically redirected back to your app. The `onResponse` and `onResponseBytes` props will allow you to get the response containing the proofs.

To get the response of your users after his redirection without using the `SismoConnectButton` you can also use the `useSismoConnect` hook. This allows you to redirect your users to a page other than the page with the button integrated.

```typescript
import { useSismoConnect, SismoConnectClientConfig } from "@sismo-core/sismo-connect-react";

const config: SismoConnectClientConfig = { appId: "0xf4977993e52606cfd67b7a1cde717069" };

const { response, responseBytes } = useSismoConnect({ config });
```

The `useSismoConnect` hook also exposes the Sismo Connect variable which provides access to all the features available in the [Sismo Connect Client package](client.md).

```typescript
const { sismoConnect } = useSismoConnect({ config });
```

Please see the [Sismo Connect Client documentation](client.md) to see all the possibilities around the Sismo Connect instance.

## Documentation

This package is a wrapper of the Sismo Connect Client package, which means that all the core concepts such as `SismoConnectClientConfig`, `AuthRequest`,`ClaimRequest`, `SignatureRequest`, and `SismoConnectResponse` are the same as in the client package. If you need to explore these concepts in detail, we advise you to refer to the [Sismo Connect Client](client.md) page, which provides more detailed information.

### SismoConnectButton

The SismoConnectButton component will allow you to easily request proofs from your user for a groupId they should be a member of.

```typescript
<SismoConnectButton 
    // the client config created
    config={config}
    // request a proof of account ownership 
    // (here Vault ownership)
    auth={{authType: AuthType.VAULT}}
    // if you need mutiple auth requests
    auths={[...]}
    // request a proof of group membership 
    // (here the group with id 0x42c768bb8ae79e4c5c05d3b51a4ec74a)
    claim={{groupId: "0x42c768bb8ae79e4c5c05d3b51a4ec74a"}}
    // if you need multiple claim requests
    claims={[...]}
    // request a message signature
    signature={{message: "Your message"}}
    // a boolean putting the button in loading mode if set to true
    loading={loading}
    // Some text to display on the button
    text={text}
    // gets users response holding proofs when users are redirected from the vault app
    onResponse={async (response: SismoConnectResponse) => {
	//Send the response to your server to verify it
	//thanks to the @sismo-core/sismo-connect-server package
    }}
    // gets users response holding proofs when users are redirected from the vault app
    // the response is encoded as bytes to easily send to smart contracts
    onResponseBytes={async (bytes: string) => {
        //Send the response to your contract to verify it
        //thanks to the @sismo-core/sismo-connect-solidity package
    }}
    // url to redirect your users to when their proofs are generated
    callBackUrl={"https://my-path.xyz"}
/>
```

**Props**

| Prop                                                                  | Type                                    | Description                                                                                                                                                                                |
| --------------------------------------------------------------------- | --------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| config _(optional but must at least provide this config or an appId)_ | SismoConnectClientConfig                | The config allows you to fully customize your Sismo Connect integration.                                                                                                                   |
| appId _(optional but must at least provide this appId or a config)_   | string                                  | appId of your Sismo Connect app. Go on the [Sismo Factory](https://factory.sismo.io/apps-explorer) to register your appId.                                                                 |
| auth                                                                  | AuthRequest                             | The AuthRequest object holds all the information needed to generate a proof of account ownership.                                                                                          |
| auths                                                                 | AuthRequest\[]                          | Aggregation of AuthRequest                                                                                                                                                                 |
| claim                                                                 | ClaimRequest                            | The ClaimRequest object holds all the information needed to generate a proof of group membership.                                                                                          |
| claims                                                                | ClaimRequest\[]                         | Aggregation of ClaimRequest                                                                                                                                                                |
| signature                                                             | string                                  | Message to sign in the Vault                                                                                                                                                               |
| loading _(optional)_                                                  | boolean                                 | This property allows you to disable the button and replace the badge icon with a loader.                                                                                                   |
| **\[deprecated**_**]**_ verifying _(optional)_                        | boolean                                 | This prop allows you to trigger the loading of the button.                                                                                                                                 |
| text                                                                  | string                                  | This property allows to replace the text of the button.                                                                                                                                    |
| onResponse _(optional)_                                               | (response: SismoConnectResponse) ⇒ void | Return a response that contains the proofs generated by your users that must be sent to your backend and verified thanks to the @sismo-core/sismo-connect-server package.                  |
| onResponseBytes _(optional)_                                          | (bytes: string) => void                 | Return a response in bytes usable in your contracts.                                                                                                                                       |
| callbackUrl _(optional)_                                              | string                                  | By default, the redirection will send you back to the same page as your button. The callback url props will send your user to another url. See useSismoConnect to get the user response.   |
| **\[deprecated**_**]**_ callbackPath _(optional)_                     | string                                  | By default, the redirection will send you back to the same page as your button. The callback path props will send your user to another path. See useSismoConnect to get the user response. |
| overrideStyle                                                         | React.CSSProperties                     | Override style of the Sismo Connect                                                                                                                                                        |

**Customization**

If you want to customize the style of your button, you can use 3 CSS classes `SismoConnectButton`, `sismoConnectButtonText`, and `sismoConnectButtonLogo`.

An overrideStyle prop is also available on the component which allows you to override the same style as the class `sismoConnectButton`.

Here is an example of a custom button:

```css
//Added in the index.css of a React app

.sismoConnectButton {
	background: red !important;
}
.sismoConnectButtonText {
	color: blue !important;
}
.sismoConnectButtonLogo {
	display: none
}
```

#### useSismoConnect

The `useSismoConnect` hook makes it easy for you to obtain the response containing the user proof, and it provides access to all functions exposed by the Sismo Connect client instance.

```typescript
import { useSismoConnect } from "@sismo-core/sismo-connect-react";

const { useSismoConnect, response, responseBytes } = useSismoConnect({ config });
```

**Params**

| Param  | Type                     | Description                                                                                |
| ------ | ------------------------ | ------------------------------------------------------------------------------------------ |
| config | SismoConnectClientConfig | The SismoConnectClientConfig allows you to fully customize your Sismo Connect integration. |

**States returned**

| State         | Type                         | Description                                                                                                                                                     |
| ------------- | ---------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| response      | SismoConnectResponse \| null | The response contains the proofs generated by your users that must be sent to your backend and verified thanks to the @sismo-core/sismo-connect-server package. |
| responseBytes | string \| null               | The response contains the proofs generated by your users that must be sent to your contract.                                                                    |
| sismoConnect  | SismoConnectClient           | Sismo Connect instance with the client configuration.                                                                                                           |
