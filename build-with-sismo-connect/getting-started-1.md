# Installation

<details>

<summary>Supported EVM Chains</summary>

**Mainnets**

* **Mainnet** (1)
* **Optimism** (10)
* **Gnosis** (100)
* **Polygon** (137)
* **Base** (8453)
* **Arbitrum One** (42161)

**Testnets**

* **Arbitrum Goerli** (421613)
* **Goerli** (5)
* **BaseGoerli** (84531)
* **Mumbai** (80001)
* **Optimism Goerli** (420)
* **Scroll Alpha Testnet** (534353) **\[deprecated]**
* **Sepolia** (11155111)

</details>

## Get your appId - (30 secs)

Before anything, go to the [Sismo Factory](https://factory.sismo.io) and create your app. Once your app is created, make sure to get your appId.

## Quick start - (1 min)

Choose and install the starter of your choice in one unique command line.

<pre class="language-bash"><code class="lang-bash">npx create-sismo-connect-app@latest
# or
<strong>npm create sismo-connect-app@latest
</strong># or
yarn create sismo-connect-app
</code></pre>

This command enables to you install one of the available starters:

1. **offchain: Sismo Connect +** [**Next.js**](https://nextjs.org/docs)\
   request ZK Proofs from users and verify them in a backend
2. **onchain: Sismo Connect +** [**Next.js**](https://nextjs.org/docs) **+** [**Foundry**](https://getfoundry.sh/) **+** [**wagmi**](https://wagmi.sh/)\
   request ZK Proofs from users and verify them in a smart contract
3. **\[Upcoming] onchain: Sismo Connect + Next.js + hardhat + ethers**\
   coming very soon, until then, head over the [Manual Installation](getting-started-1.md#manual-installation) if you want to use Sismo Connect with hardhat

Feel free to check the [Sismo Connect Cheatsheet](sismo-connect-cheatsheet.md), a great companion when developing an app using Sismo Connect.

{% hint style="success" %}
We are here to support you on our [builders telegram group](https://builders.sismo.io)
{% endhint %}

## Manual Installation

This section is intended for developers who have prior experience with incorporating new tools into their existing repositories.

### Integrate Sismo Connect in Your Front End

Make a Sismo Connect Request, users will be redirected to their Data Vault to generate a ZK proof and send your front end a Sismo Connect Response.\
This response, containing the ZK proof, will be verified on your back end/smart contract.

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
            "leo21.sismo.eth",
            "0xA4C94A6091545e40fc9c3E0982AEc8942E282F38",
            "0x1b9424ed517f7700e7368e34a9743295a225d889",
            "0x82fbed074f62386ed43bb816f748e8817bf46ff7",
            "0xc281bd4db5bf94f02a8525dca954db3895685700",
            // Github
            "github:leo21",
            // Twitter
            "twitter:leo21_eth",
            // Telegram
            "telegram:leo21",
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
      // retrieve the Sismo Connect Reponse from the user's Sismo data vault
      onResponse={async (response: SismoConnectResponse) => {
        const res = await fetch("/api/verify", {
          method: "POST",
          body: JSON.stringify(response),
        });
        console.log(await res.json());
      }}
      // reponse in bytes to call a contract
      // onResponseBytes={async (response: string) => {
      //   console.log(response);
      // }}
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

### Sismo Connect in Your Smart Contracts/Back Ends

Your back end/smart contract will receive a Sismo Connect Response forwarded from your front end that you must verify.

1. Install the Sismo Connect Library

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
forge install sismo-core/sismo-connect-solidity --no-commit
```

Add the remapping in `remappings.txt`:

```bash
echo $'sismo-connect-solidity/=lib/sismo-connect-solidity/src/' >> remappings.txt
```
{% endtab %}

{% tab title="Hardhat" %}
```bash
# install the package
yarn add @sismo-core/sismo-connect-solidity
```

**Import the library**

In your Solidity file:

```solidity
import "@sismo-core/sismo-connect-solidity/contracts/SismoConnectLib.sol";
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

2. verify Sismo Connect responses sent from your front end

{% hint style="warning" %}
The Sismo Connect configuration and request used in your smart contract/backend must exactly match those from your frontend.
{% endhint %}

{% tabs %}
{% tab title="Onchain - Verify in a Smart Contract " %}
```solidity
// in src/Airdrop.sol of a Foundry project - https://book.getfoundry.sh/getting-started/first-steps
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "sismo-connect-solidity/SismoConnectLib.sol";

// This is a sample contract that shows how to use the SismoConnect library
contract Airdrop is SismoConnect {
    event ResponseVerified(SismoConnectVerifiedResult result);

    constructor()
        SismoConnect(
            buildConfig({
                // replace with your appId from the Sismo factory https://factory.sismo.io/
                // should match the appId used to generate the response in your frontend
                appId: 0xf4977993e52606cfd67b7a1cde717069,
                // For development purposes insert when using proofs that contains impersonation
                // Never use this in production
                isImpersonationMode: true
            })
        )
    {}

    function verifySismoConnectResponse(bytes memory response) public {
        // build the auth and claim requests that should match the response
        AuthRequest[] memory auths = new AuthRequest[](1);
        auths[0] = buildAuth({authType: AuthType.GITHUB});

        ClaimRequest[] memory claims = new ClaimRequest[](2);
        // ENS DAO Voters
        claims[0] = buildClaim({groupId: 0x85c7ee90829de70d0d51f52336ea4722});
        // Gitcoin passport with at least a score of 15
        claims[1] = buildClaim({
            groupId: 0x1cde61966decb8600dfd0749bd371f12,
            value: 15,
            claimType: ClaimType.GTE
        });

        // verify the response regarding our original request
        SismoConnectVerifiedResult memory result = verify({
            responseBytes: response,
            auths: auths,
            claims: claims,
            signature: buildSignature({message: "I vote Yes to Privacy"})
        });

        emit ResponseVerified(result);
    }
}

```
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
        "leo21.sismo.eth",
        "0xa4c94a6091545e40fc9c3e0982aec8942e282f38",
        // Github
        "github:leo21",
        // Twitter
        "twitter:leo21_eth",
        // Telegram
        "telegram:leo21",
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
    return NextResponse.json(result, { status: 200 });
  } catch (e: any) {
    console.error(e);
    return NextResponse.json(e.message, { status: 500 });
  }
}

```

{% hint style="success" %}
If you are using Nextjs, you will need to add this config in the `next.config.js` file to be able to verify the proof. You can find more information [here](https://nextjs.org/docs/app/api-reference/next-config-js/serverComponentsExternalPackages).

```javascript
/** @type {import('next').NextConfig} */
const nextConfig = {
  experimental: {
    serverComponentsExternalPackages: ["@sismo-core/sismo-connect-server"],
  },
}

module.exports = nextConfig
```
{% endhint %}
{% endtab %}
{% endtabs %}

