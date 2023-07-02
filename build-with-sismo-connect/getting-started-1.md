# Quickstart

This section targets developers experienced with installation of new tools to their own repositories.

&#x20;If you have difficulties, head over to the pre-configured [boilerplates](run-example-apps/) or [tutorials](tutorials/). Neither demand experience in our tech stack to get you setup.

## Step 1 - Setup: Create your Sismo Connect App

You must create a Sismo Connect App in the [Sismo Factory](https://factory.sismo.io) ([tutorial](tutorials/create-a-sismo-connect-app.md)) and get your appId. this appId will be required both in your frontend and smart contracts/ backend.

## Step 2 - Request: Integrate Sismo Connect in your frontend

Your frontend must make a Sismo Connect request, users will be redirected to their Data Vault to generate a ZK Proof and your frontend will receive a Sismo Connect Response from them. \
This response, containing the ZK Proof, will be verified on your backend/smart contract.

1. Install our React Library

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

{% hint style="info" %}
<mark style="color:purple;">`@sismo-core/sismo-connect-client`</mark> is also available for non react frontend. ([docs](technical-documentation/packages/client.md))
{% endhint %}

2. Create your Sismo Connect config

```typescript
// sismo-connect-config.ts

import { SismoConnectConfig } from "@sismo-core/sismo-connect-react";

export const config: SismoConnectConfig = {
  appId: "0xf4977993e52606cfd67b7a1cde717069", // replace with your appId
  // vault: {
  //   // For development purposes insert the Data Sources that you want to impersonate here
  //   // Never use this in production
  //   impersonate: [
  //     // EVM
  //     "dhadrien.sismo.eth",
  //     "leo21.sismo.eth",
  //     "0xa4c94a6091545e40fc9c3e0982aec8942e282f38",
  //     "vitalik.eth",
  //     // Github
  //     "github:dhadrien",
  //     // Twitter
  //     "twitter:dhadrien_",
  //     // Telegram
  //     "telegram:dhadrien",
  //   ],
  // },
  // displayRawResponse: true,

};
```

{% hint style="info" %}
[Learn more](technical-documentation/sismo-connect-configuration.md) about Sismo Connect config and impersonation mode
{% endhint %}

3. Use our React Button to make Sismo Connect Requests

```typescript
// Component.tsx

import { SismoConnectButton, AuthType, SismoConnectResponse } from "@sismo-core/sismo-connect-react";
import { config } from "./sismo-connect-config.ts";

<SismoConnectButton
    config={config}
    // request proof of Data Sources ownership (e.g EVM, GitHub, twitter or telegram)
    auths={[{ authType: AuthType.GITHUB }]}
    // request zk proof that Data Source are part of a group
    // (e.g NFT ownership, Dao Participation, GitHub commits)
    claims={[{groupId: ENS_DAO_VOTERS_GROUP_ID}]}
    // request message signature from users.
    signature={message: "I vote Yes to Privacy"}}
    onResponseBytes={(response: string) => {
        // call your contract/ backend with the response as bytes
    }}
/>
```

{% hint style="success" %}
Check the [Sismo Connect Example Requests](sismo-connect-cheatsheet.md) to get a large set of interesting requests
{% endhint %}

## Step 3 - Verify: Sismo Connect in your Smart Contracts/ Backends

Your backend/smart contract will receive a Sismo Connect Response forwarded from your frontend that you must verify.

1. Install the Sismo Connect Library

<details>

<summary>EVM Chains supported</summary>

#### Mainnets

* **Arbitrum One** (42161)
* **Gnosis** (100)
* **Mainnet** (1)
* **Optimism** (10)
* **Polygon** (137)

#### Testnets

* **Arbitrum Goerli** (421613)
* **Goerli** (5)
* **Mumbai** (80001)
* **Optimism Goerli** (420)
* **Scroll Alpha Testnet** (534353)
* **Sepolia** (11155111)

</details>

{% tabs %}
{% tab title="Onchain - Verify in a Smart Contract " %}
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
{% endtab %}

{% tab title="Offchain - Verify in a Backend" %}
Install the TypeScript library

{% tabs %}
{% tab title="yarn" %}
```bash
yarn add @sismo-core/sismo-connect-server
```
{% endtab %}
{% endtabs %}
{% endtab %}
{% endtabs %}

2. Reuse your Sismo Connect config and verify Sismo Connect responses sent from your frontend

{% hint style="warning" %}
The config you use in your smart contract/ backend must exactly match the one from your frontend.
{% endhint %}

{% tabs %}
{% tab title="Onchain - Verify in a Smart Contract " %}
<pre class="language-solidity"><code class="lang-solidity">import "sismo-connect-solidity/SismoLib.sol";

contract Airdrop is SismoConnect {
  // reference your appId
  bytes16 public constant APP_ID = 0xf4977993e52606cfd67b7a1cde717069;

  constructor()
    // use buildConfig helper to easily build a Sismo Connect config in Solidity
    SismoConnect(buildConfig(APP_ID))
  {}
<strong>
</strong><strong>  function verifySismoConnectResponse(bytes memory response) public {
</strong><strong>    SismoConnectVerifiedResult memory result = verify({
</strong>        responseBytes: response,
        auths: auths,
        claims: claims,
        signature: signature
    });

    // implement some logic if the proof is successful
  }
}
</code></pre>
{% endtab %}

{% tab title="Offchain - Verify in a Backend" %}
Reuse the Sismo Connect configuration from your frontend

```solidity
// sismo-connect-config.ts

import { SismoConnectConfig } from "@sismo-core/sismo-connect-server";

export const config: SismoConnectConfig = {
  appId: "0xf4977993e52606cfd67b7a1cde717069", // replace with your appId
};
```

Create an api route to receive the Sismo Connect response and verify it.

```typescript
// my-api.ts
import { SismoConnect, SismoConnectVerifiedResult } from "@sismo-core/sismo-connect-server";
import { config } from "./sismo-connect-config.ts";

const sismoConnect = SismoConnect(sismoConnectConfig);

// NODEJS + EXPRESS API ENDPOINT EXAMPLE
app.post('/api', (req, res) => {
  const { response } = req.body;
  const result: SismoConnectVerifiedResult = await sismoConnect.verify(response, {
      / request proof of Data Sources ownership (e.g EVM, GitHub, twitter or telegram)
    auths={[{ authType: AuthType.GITHUB }]}
    // request group membership (e.g NFT ownership, Dao Participation, GitHub commits)
    claims=[{{groupId: ENS_DAO_VOTERS_GROUP_ID}}]
    // request message signature from users.
    signature={message: "I vote Yes to Privacy"}}
   },
    );
    
  // some logic if the verification is successful
  
});

```

```typescript
// verify.ts

import { SismoConnect, SismoConnectVerifiedResult } from "@sismo-core/sismo-connect-server";
import { config } from "./sismo-connect-config.ts";

const sismoConnect = SismoConnect(sismoConnectConfig);
```
{% endtab %}
{% endtabs %}

