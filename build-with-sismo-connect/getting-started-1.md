# Quickstart

This section is intended for developers who have prior experience with incorporating new tools into their existing repositories.

{% hint style="info" %}
Having difficulties? Head over to the pre-configured [boilerplates](run-example-apps/) or [tutorials](tutorials/). Neither demand experience with Sismo's tech stack to get set up.
{% endhint %}

## Step 1 - Setup: Create Your Sismo Connect App

You must create a Sismo Connect app in the [Sismo Factory](https://factory.sismo.io) ([tutorial](tutorials/create-a-sismo-connect-app.md)) and get your appId. This appId will be required both in your front end and smart contracts/back end.

## Step 2 - Request: Integrate Sismo Connect in Your Front End

Your front end must make a Sismo Connect request for users to be redirected to their Data Vault to generate a ZK proof and send your front end a Sismo Connect Response. This response, containing the ZK proof, will be verified on your back end/smart contract.

{% hint style="success" %}
Check the [Sismo Connect Cheatsheet](sismo-connect-cheatsheet.md) to see examples of requests.
{% endhint %}

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
<mark style="color:purple;">`@sismo-core/sismo-connect-client`</mark> is also available for non-React front ends. ([docs](technical-documentation/packages/client.md))
{% endhint %}

2. Use our React Button to make Sismo Connect Requests

```typescript
// Next.js https://nextjs.org/docs/getting-started/installation
// in src/page.tsx 
"use client";

import {
  SismoConnectButton,
  AuthType,
  SismoConnectResponse,
  ClaimType,
} from "@sismo-core/sismo-connect-react";

export default function Home() {
  return (
    <SismoConnectButton
      config={{
        appId: "0xf4977993e52606cfd67b7a1cde717069", // replace with your appId
        vault: {
          // For development purposes insert the Data Sources that you want to impersonate here
          // Never use this in production
          impersonate: [
            // EVM
            "dhadrien.sismo.eth",
            "0xa4c94a6091545e40fc9c3e0982aec8942e282f38",
            // Github
            "github:dhadrien",
            // Twitter
            "twitter:dhadrien_",
            // Telegram
            "telegram:dhadrien",
          ],
        },
        // displayRawResponse: true,
      }}
      // request proof of Data Sources ownership (e.g EVM, GitHub, twitter or telegram)
      auths={[{ authType: AuthType.GITHUB }]}
      // request zk proof that Data Source are part of a group
      // (e.g NFT ownership, Dao Participation, GitHub commits)
      claims={[
        // ENS DAO Voters
        { groupId: "0x85c7ee90829de70d0d51f52336ea4722" }, 
        // Gitcoin passport with at least a score of 15
        { groupId: "0x1cde61966decb8600dfd0749bd371f12", value: 15, claimType: ClaimType.GTE }
      ]} 
      // request message signature from users.
      signature={{ message: "I vote Yes to Privacy" }}
      onResponse={async (response: SismoConnectResponse) => {
        const res = await fetch("/api/verify", {
          method: "POST",
          body: JSON.stringify(response),
        });
        console.log(await res.json());
        // to call contracts
        // onResponseBytes={async (response: SismoConnectResponse) => {
        //   await myContract.claimWithSismo(response.responseBytes);
        // }
      }}
    />
  );
}
```

{% hint style="success" %}
Check the [Sismo Connect Cheatsheet ](sismo-connect-cheatsheet.md)to get a large set of interesting requests.
{% endhint %}

{% hint style="info" %}
[Learn more](technical-documentation/sismo-connect-configuration.md) about Sismo Connect config and impersonation mode.
{% endhint %}

## Step 3 - Verify: Sismo Connect in Your Smart Contracts/Back Ends

Your back end/smart contract will receive a Sismo Connect Response forwarded from your front end that you must verify.

1. Install the Sismo Connect Library

<details>

<summary>Supported EVM Chains</summary>

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

Install the Forge dependency:

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

In your Solidity file:

```solidity
import "@sismo-core/sismo-connect-solidity/contracts/libs/SismoLib.sol";
```
{% endtab %}
{% endtabs %}
{% endtab %}

{% tab title="Offchain - Verify in a Back End" %}
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

2. Reuse your Sismo Connect config and verify Sismo Connect responses sent from your front end

{% hint style="warning" %}
The config you use in your smart contract/backend must exactly match the one from your front end.
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

{% tab title="Offchain - Verify in a Back End" %}
Create an API route to receive the Sismo Connect response and verify it.

```typescript
// Next.js API route support: https://nextjs.org/docs/api-routes/introduction
// in src/app/api/verify/route.ts

import {
  AuthType,
  ClaimType,
  SismoConnect,
  SismoConnectVerifiedResult,
} from "@sismo-core/sismo-connect-server";
import { NextResponse } from "next/server";

const sismoConnect = SismoConnect({
  config: {
    appId: "0xf4977993e52606cfd67b7a1cde717069",
    vault: {
      // For development purposes insert the Data Sources that you want to impersonate here
      // Never use this in production
      impersonate: [
        // EVM
        "dhadrien.sismo.eth",
        "0xa4c94a6091545e40fc9c3e0982aec8942e282f38",
        // Github
        "github:dhadrien",
        // Twitter
        "twitter:dhadrien_",
        // Telegram
        "telegram:dhadrien",
      ],
    },
  },
});

// this is the API route that is called by the SismoConnectButton
export async function POST(req: Request) {
  const sismoConnectResponse = await req.json();
  try {
    // verify the sismo connect response that corresponds to the request
    const result: SismoConnectVerifiedResult = await sismoConnect.verify(sismoConnectResponse, {
      auths: [{ authType: AuthType.GITHUB }],
      claims: [
        // ENS DAO Voters
        { groupId: "0x85c7ee90829de70d0d51f52336ea4722" }, 
        // Gitcoin passport with at least a score of 15
        { groupId: "0x1cde61966decb8600dfd0749bd371f12", value: 15, claimType: ClaimType.GTE },
      ],
      // verify signature from users.
      signature: { message: "I vote Yes to Privacy" },
    });
    return NextResponse.json(JSON.stringify(result), { status: 200 });
  } catch (e: any) {
    console.error(e);
    return NextResponse.json(e.message, { status: 500 });
  }
}

```
{% endtab %}
{% endtabs %}
