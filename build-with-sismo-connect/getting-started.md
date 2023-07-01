# Getting Started

Sismo Connect, a crypto-native single sign-on (SSO), enables applications to request any data aggregated in a user’s [Data Vault](../how-sismo-works/technical-concepts/what-is-the-data-vault.md). More precisely, applications can request that users prove ownership of [Data Sources](../how-sismo-works/core-components.md#what-are-data-sources) (EVM, Twitter, Telegram and GitHub accounts) and [Data Gems](../how-sismo-works/core-components.md#what-are-data-gems) (e.g. that they own a certain NFT or voted in a DAO). By abstracting the underlying complexities, Sismo Connect allows developers to implement zero-knowledge technology in their applications with only a few lines of code.

{% tabs %}
{% tab title="Onchain" %}
If you wish to fork a repository with everything set up properly, you can fork our [**onchain boilerplate**](run-example-apps/onchain-sample-project.md).

## Prerequisites

* [Node.js](https://nodejs.org/en/download/) >= 18.15.0 (Latest LTS version)
* [Yarn](https://classic.yarnpkg.com/en/docs/install)
* [Foundry](https://book.getfoundry.sh/) (see how to install it [here](https://book.getfoundry.sh/getting-started/installation))&#x20;
* MetaMask installed in your browser

## Front end

### Installation

Install the Sismo Connect React package:

{% tabs %}
{% tab title="yarn" %}
```bash
yarn add @sismo-core/sismo-connect-react
```
{% endtab %}

{% tab title="npm" %}
```bash
npm install @sismo-core/sismo-connect-react
```
{% endtab %}
{% endtabs %}

### Create a Sismo Connect App in the Factory

[**Creating a Sismo Connect App in the Factory**](tutorials/create-a-sismo-connect-app.md) will give you an application Id (`appId`) that is used to configure Sismo Connect in your front end and smart contracts/back end.

### Sismo Connect configuration

Create a Sismo Connect configuration in a file of your choice and export it. You need to [create an app in the Sismo Factory](tutorials/create-a-sismo-connect-app.md) to get an `appId`.

```typescript
// sismo-connect-config.ts

import { SismoConnectConfig } from "@sismo-core/sismo-connect-react";

export const config: SismoConnectConfig = {
  appId: "0xf4977993e52606cfd67b7a1cde717069", // replace with your appId
};
```

### Request proofs from your users

Request proofs from your users by creating a Sismo Connect React Button, pass the config created earlier as a prop and implement the logic to call your contract with the response (as bytes) from a user's Data Vault.&#x20;

```typescript
// Component.tsx

import { SismoConnectButton, AuthType, SismoConnectResponse } from "@sismo-core/sismo-connect-react";
import { config } from "./sismo-connect-config.ts";

<SismoConnectButton
    config={config}
    // request proof of Data Sources ownership (e.g EVM, GitHub, twitter or telegram)
    auths={[{ authType: AuthType.GITHUB }]}
    // request proof of Data Gems (e.g NFT ownership, Dao Participation, GitHub commits)
    claims=[{{groupId: ENS_DAO_VOTERS_GROUP_ID}}]
    // request message signature from users.
    signature={message: "I vote Yes to Privacy"}}
    onResponseBytes={(response: string) => {
        // call your contract with the response as bytes
    }
/>
```



## Smart Contract

You can see the chains where Sismo Connect is available [**here**](../how-sismo-works/resources/sismo-101.md).

### Installation

{% tabs %}
{% tab title="Foundry (recommended)" %}
{% hint style="info" %}
Make sure to have [Foundry](https://book.getfoundry.sh/getting-started/installation) installed.
{% endhint %}

Install the forge dependency:

```bash
foundryup
forge install sismo-core/sismo-connect-packages
```

Add the remapping in remappings.txt:

```bash
echo $'sismo-connect-solidity/=lib/sismo-connect-packages/packages/sismo-connect-solidity/src/' >> remappings.txt
```

#### Import the library

In your solidity file:

```solidity
import "sismo-connect-solidity/SismoLib.sol"; 
```
{% endtab %}

{% tab title="Hardhat" %}
```bash
# install the package
yarn add @sismo-core/sismo-connect-solidity
```

#### Import the library

In your solidity file:

```solidity
import "@sismo-core/sismo-connect-solidity/contracts/libs/SismoLib.sol";
```
{% endtab %}
{% endtabs %}

### Sismo Connect configuration

In your smart contracts, you can create a Sismo Connect Configuration simply by passing an `appId` in the constructor and using the `buildConfig` helper.

```solidity
import "sismo-connect-solidity/SismoLib.sol";

contract Foo is SismoConnect {
  // reference your appId
  bytes16 public constant APP_ID = 0xf4977993e52606cfd67b7a1cde717069;

  constructor()
    // use buildConfig helper to easily build a Sismo Connect config in Solidity
    SismoConnect(buildConfig(APP_ID))
  {}
}
```

### Verify proofs from your users

Simply call the verify function with the response as bytes.&#x20;

<pre class="language-solidity"><code class="lang-solidity"><strong>function foo(bytes memory response) public {
</strong><strong>    SismoConnectVerifiedResult memory result = verify({
</strong>        responseBytes: response,
        auths: auths,
        claims: claims,
        signature: signature
    });

    // implement some logic if the proof is successful
}
</code></pre>
{% endtab %}

{% tab title="Offchain" %}
If you wish to fork a repository with everything set up properly, you can fork our [**offchain boilerplate**](run-example-apps/offchain-sample-project.md).

## Prerequisites

* [Node.js](https://nodejs.org/en/download/) >= 18.15.0 (Latest LTS version)
* [Yarn](https://classic.yarnpkg.com/en/docs/install)

## Frontend

### Installation

Install Sismo Connect React package:

{% tabs %}
{% tab title="yarn" %}
```bash
yarn add @sismo-core/sismo-connect-react
```
{% endtab %}

{% tab title="npm" %}
```bash
npm install @sismo-core/sismo-connect-react
```
{% endtab %}
{% endtabs %}

### Create a Sismo Connect App in the Factory

[**Creating a Sismo Connect App in the Factory**](tutorials/create-a-sismo-connect-app.md) will give you an application Id (`appId`) that is used to configure Sismo Connect in your front end and smart contracts/back end.

### Sismo Connect configuration

Create a Sismo Connect configuration in a file of your choice and export it. You need to [create an app in the Sismo Factory](tutorials/create-a-sismo-connect-app.md) to get an `appId`.

```typescript
// sismo-connect-config.ts

import { SismoConnectConfig } from "@sismo-core/sismo-connect-react";

export const config: SismoConnectConfig = {
  appId: "0xf4977993e52606cfd67b7a1cde717069", // replace with your appId
};
```

### Request proofs from your users

Request proofs from your users by creating a Sismo Connect React Button, pass the config created earlier as a prop and implement the logic to call your backend with the response from a user's Data Vault.&#x20;

```typescript
// Component.tsx

import { SismoConnectButton, AuthType, SismoConnectResponse } from "@sismo-core/sismo-connect-react";
import { config } from "./sismo-connect-config.ts";

<SismoConnectButton
    config={config}
    // request proof of Twitter ownership
    auths={[{ authType: AuthType.TWITTER }]}
    onResponse={(response: SismoConnectResponse) => {
        // call your backend with the response
      await axios.post(`/api`, {
        response,
      }});
    }
/>
```



## Back end

### Installation

Install Sismo Connect React package:

{% tabs %}
{% tab title="yarn" %}
```bash
yarn add @sismo-core/sismo-connect-server
```
{% endtab %}

{% tab title="npm" %}
```bash
npm install @sismo-core/sismo-connect-server
```
{% endtab %}
{% endtabs %}

### Sismo Connect configuration

If you have your front end and back end in the same repository, you can simply use the configuration you created earlier for the front end. Otherwise, simply create the same configuration in a file for your back end.

### Verify proofs from your users

Create an endpoint in your backend to receive a Sismo Connect Response and verify it.

```typescript
// my-api.ts
import { SismoConnect, SismoConnectVerifiedResult } from "@sismo-core/sismo-connect-server";
import { config } from "./sismo-connect-config.ts";

const sismoConnect = SismoConnect(sismoConnectConfig);

// NODEJS + EXPRESS API ENDPOINT EXAMPLE
app.post('/api', (req, res) => {
  const { response } = req.body;
  const result: SismoConnectVerifiedResult = await sismoConnect.verify(response, {
      auths: [{ authType: AuthType.TWITTER }],
    });
    
  // some logic if the verification is successful
  
});
```
{% endtab %}
{% endtabs %}
