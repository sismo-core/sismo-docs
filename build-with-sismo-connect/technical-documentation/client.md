---
description: Request proofs from your user
---

# Sismo Connect Client: Request

The [Sismo Connect](../../discover-sismo-connect/empower-your-app.md) Client is a frontend package built on top of the [Sismo Data Vault](../../what-is-sismo/personal-data-sismos-data-vault-gems-and-groups.md) app (the prover) to easily request proofs from your users with [AuthRequests](client.md#authrequest), [ClaimRequests](client.md#claimrequest) and [SignatureRequests](client.md#signaturerequest).

## Installation

Install the Sismo Connect Client package in your frontend with `npm` or `yarn`:

```bash
# with npm
npm install @sismo-core/sismo-connect-client
# with yarn
yarn add @sismo-core/sismo-connect-client
```

{% hint style="success" %}
Make sure to have at least v18.15.0 as Node version. You can encounter issues with ethers dependencies if not.
{% endhint %}

## Usage

### Configuration

The first step for integrating Sismo Connect in your frontend is to create a `SismoConnectClientConfig`. This config will require an `appId` and can be customized with [optional fields.](client.md#sismoconnectclientconfig) You can go to the [Sismo Factory](../../sismo-factory/create-a-sismo-connect-app.md) to create a Sismo Connect App and get an `appId` ([here is a tutorial](../../sismo-factory/create-a-sismo-connect-app.md)).

```typescript
import { SismoConnect, SismoConnectClientConfig } from "@sismo-core/sismo-connect-client";

const config: SismoConnectClientConfig = {
  // you will need to get an appId from the Factory
  appId: "0xf4977993e52606cfd67b7a1cde717069", 
}

// create a new SismoConnect instance with the client configuration
const sismoConnect = SismoConnect(config);
```

### Request proofs of account ownership from your users

Request proofs of account ownership from your users by creating an `AuthRequest` and calling the `request` function of the Sismo Connect Client. Only the `authType` in the `AuthRequest` is mandatory.&#x20;

You can request proofs of:

* **Data Vault** ownership (AuthType.**VAULT**)
* **EVM account** ownership (AuthType.**EVM\_ACCOUNT**)
* **Github account** ownership (AuthType.**GITHUB**)
* **Twitter account** ownership (AuthType.**TWITTER**)

<pre class="language-typescript"><code class="lang-typescript">import { AuthType, AuthRequest } from "@sismo-core/sismo-connect-client";

<strong>const auth: AuthRequest = { 
</strong><strong>    // user should prove that they own a EVM account
</strong>    authType: AuthType.EVM_ACCOUNT,
};
// The `request` function sends your user to the Sismo Vault App 
// to generate the proof of account ownership
// After the proof generation, the user is redirected with it to your app
sismoConnect.request({ auth: auth });

// you can request users to prove ownership of an EVM AND a Twitter account
// By making multiple auth requests:
const secondAuth: AuthRequest = { authType: AuthType.TWITTER };
sismoConnect.request({ auths: [auth, secondAuth] });
</code></pre>

### Request proofs of group membership from your users

Request proofs of group membership from your users by creating a `ClaimRequest` and calling the `request` function of the Sismo Connect Client. Only the `groupId` in the `ClaimRequest` is mandatory.

```typescript
import { ClaimRequest } from "@sismo-core/sismo-connect-client";

// claim 1: request a proof of ENS handle ownership
const claim: ClaimRequest = { 
    // ID of the group the user should be a member of
    // here: ens-domains-holders
    groupId: "0x8b64c959a715c6b10aa8372100071ca7",
};

// The `request` function sends your user to the Sismo Vault App 
// to generate the proof of group membership
// After the proof generation, the user is redirected with it to your app
sismoConnect.request({ claim: claim });
```

You can ask users to **aggregate their data** by proving that they are human AND that they have an ENS. To do this, we can include several claims in our request:

```typescript
// claim 2: request a proof of PoH ownership
const secondClaim: ClaimRequest = { groupId: "0x682544d549b8a461d7fe3e589846bb7b" };

// Request of multiple claims:
sismoConnect.request({ claims: [claim, secondClaim] });
```

### Request a message signature from your users

Request to embed a message in your users proof by creating a `SignatureRequest` and calling the `request` function of the Sismo Connect client. When you create a `SignatureRequest`, users will embed this message in their proofs, it is then possible to verify if this message is present in the proof provided to your app when verifying in your smart contract or your backend.

<pre class="language-typescript"><code class="lang-typescript">import { SignatureRequest } from "@sismo-core/sismo-connect-client";

const signature: SignatureRequest = { 
    message: "Your message here"
};

<strong>sismoConnect.request({ signature: signature });
</strong></code></pre>

### Get proofs from your users

Receive proofs in a `sismoConnectResponse` by calling the `getResponse` function when the user is redirected back to your application.

```typescript
// The `getResponse` function returns the proofs generated by your user 
// after coming back from the Sismo Vault App.
const sismoConnectResponse = sismoConnect.getResponse();
```

If you want a response that can be sent to your smart contract, you need to use the `getResponseBytes` function.

```typescript
const sismoConnectResponseBytes = sismoConnect.getResponseBytes();
```

## Documentation

### `SismoConnectClientConfig`

The `SismoConnectClientConfig` allows you to fully customize your Sismo Connect integration. Its only mandatory field is an `appId` that is generated when you create a new Sismo Connect application in the [**Sismo Factory**](https://factory.sismo.io/apps-explorer). The other fields are documented after the code snippet.

```typescript
export type SismoConnectClientConfig = {
  // the appId registered in the Factory
  appId: string,
  devMode?: {
    // will use the Dev Sismo Data Vault https://dev.vault-beta.sismo.io/
    enabled?: boolean, 
    // Display a modal at the end of the proof generation flow with the response 
    // to help you generate response for development purpose
    displayRawResponse?: boolean;
    // Override certain groups to use it in a local environment
    devGroups?: {
      groupId?: string;
      groupTimestamp?: number | "latest";
      data?: Record<string, Number | BigNumberish>
    }[]
  }
}
```

#### `devMode`

`devMode` is an object containing three fields:

* `enabled`, a boolean which states where to redirect your users to generate proofs.&#x20;
  * If set to `true`, your users will be redirected to [https://dev.vault-beta.sismo.io/](https://dev.vault-beta.sismo.io/), an isolated developer vault to experiment with dev addresses or data that they do not wish to add in their production vault for example. This developer vault works with testnet chains under the hood (Mumbai and Goerli for now).
  * If set to `false`, your users will be redirected to [https://vault-beta.sismo.io/](https://vault-beta.sismo.io/), their production vault holding valuable data. This production vault works with mainnet chains under the hood (Ethereum, Polygon and Gnosis for now).

{% hint style="info" %}
If you work with smart contracts, you should pay closely attention on which Vault you want to redirect your users as far as the **verification of proofs generated in a developer vault will fail on any mainnet chain.**
{% endhint %}

* `displayRawResponse,` a boolean indicating if you want to output the proofs in a modal instead of being redirected to your Sismo Connect application when the proof generation is successful in your Sismo Vault. This field is a good way to see your decoded proofs if you are developing with smart contracts for example.&#x20;

<figure><img src="../../.gitbook/assets/Capture d’écran 2023-05-17 à 10.22.55.png" alt="" width="563"><figcaption><p>Decoded proof in the Sismo Vault</p></figcaption></figure>

* `devGroups`, an object allowing you to specify which existing group to **override** with chosen addresses to successfully simulate group membership eligibility as a developer. While `devGroups` allows you to produce proofs you will still need to verify them in a backend OR a smart contract. To verify such proofs (made from fake groups), you will need to have the [**appropriate configuration in your backend**](server.md) OR setup a local chain if you are working with smart contract. As real groups are stored in a Merkle Tree onchain, you will need to register a fake root corresponding to your fake groups to successfully verify proofs in your smart contract.

{% hint style="info" %}
You can see how a root can be automatically registered on a local chain in this [**onchain sample repository**](https://github.com/sismo-core/sismo-connect-onchain-sample-project/blob/main/script/tree/compute-tree.ts).&#x20;
{% endhint %}

```typescript
export const sismoConnectConfig: SismoConnectClientConfig = {
  appId: "0xf4977993e52606cfd67b7a1cde717069",
  devMode: {
    // users will be redirect to their developer vault
    // at https://dev.vault-beta.sismo.io/
    enabled: true,
    devGroups: [
      {
        // Gitcoin Passport group
        // https://factory.sismo.io/groups-explorer?search=0x1cde61966decb8600dfd0749bd371f12
        // this groupId should exist 
        // it will be the group that will be overriden with the chosen data
        groupId: "0x1cde61966decb8600dfd0749bd371f12",
        // this array will override all the data of the existing group chosen
        // it will simulate group membership eligibility for the addresses defined below
        // while it allows developers to generate proofs of group membership
        // it does not mean that proofs are going to be always verified
        // you will also need to have this configuration in your backend for example
        // OR setup a local chain if you are working with smart contracts
        // 
        // IMPORTANT NOTE: the proofs generated from these addresses
        // can't be verified on testnet and mainnet chains 
        // if you are verifying with smart contracts, you must have a local chain
        // and manually register the new root for your groups manually
        data: [
          // add your dev addresses in the array
          bobDevAddress,
          aliceDevAddress,
          // other addresses to test
          "0x2b9b9846d7298e0272c61669a54f0e602aba6290",
          "0xb01ee322c4f028b8a6bfcd2a5d48107dc5bc99ec",
          "0x938f169352008d35e065F153be53b3D3C07Bcd90",
        ],
      },
    ]
  },
};
```

Here is an example of a customized `SismoConnectClientConfig` with a `devMode` enabled and two developer addresses that will be able to generate valid proofs from their developer [Data Vaults](../../what-is-sismo/personal-data-sismos-data-vault-gems-and-groups.md).

```typescript
const sismoConnectConfig: sismoConnectClientConfig = {
    // you will need to register an appId in the Factory
    appId: "0xf4977993e52606cfd67b7a1cde717069", 
    // allows to create valid proofs for specific addresses
    // should only be used when prototyping
    // default: undefined
    devMode: {
        // will use the Dev Sismo Data Vault https://dev.vault-beta.sismo.io/
        enabled: true, 
        // overrides the existing group with some developer addresses
        devGroups: [{
            // group to override with the data defined below
            groupId: "0x42c768bb8ae79e4c5c05d3b51a4ec74a",
            // data can be defined as an array of addresses
            // but also as a key-value object with address has a key
            data: {
                "0x123...abc": 1, // first developer address with value 1
                "0x456...efa": 2 // second developer address with value 2
            },
        }]
    },
}
```

### `AuthRequest`

The AuthRequest object holds all the information needed to request a proof of account ownership from your users.

```typescript
export type AuthRequest = {
  // VAULT / TWITTER / GITHUB / EVM_ACCOUNT
  authType: AuthType;
  // request a specific userId for the proof
  userId?: string;
  // Make the claim optional
  isOptional?: boolean; // default to false
  // allow the user to select the account of which he wants to prove the ownership
  isSelectableByUser?: boolean; // default to true
  // Do not reveal the userId with which the user performs the auth
  // enforced to false for now but this property will be available in the future
  // it requires some works on the circuits
  isAnon?: boolean; 
  extraData?: any; // default to ''
}

export enum AuthType {
  EMPTY,
  VAULT,
  GITHUB,
  TWITTER,
  EVM_ACCOUNT,
}
```

Here is an example of a customized `AuthRequest` that, [when requested](client.md#request), will trigger the generation of a proof of Github account ownership. The user should have a GitHub in his vault.

```typescript
import { AuthRequest } from "@sismo-core/sismo-connect-client";

const AUTH: AuthRequest = { 
    authType: AuthType.GITHUB,
};
sismoConnect.request({ auth: AUTH });
```

### `ClaimRequest`

The `ClaimRequest` object holds all the information needed to request a proof of group membership from your users.

<pre class="language-typescript"><code class="lang-typescript">export type ClaimRequest = {
    // The groupId from which the proofs are generated
    groupId?: string;
    // The timestamp of the group from which the proofs are generated
    groupTimestamp?: number | "latest"; // default to "latest"
    // The value used in the request
    value?: number; // default to 1
    // comparator used with the requested value to set group membership conditions
    // can be "GTE" or "EQ"
    claimType?: ClaimType; // default to GTE
    // Make the claim optional
    isOptional?: boolean; // default to false
    // allow users to select a value to be shared and proving their group membership
    isSelectableByUser?: boolean; // default to true
    extraData?: any; // default to ''
}

<strong>export enum ClaimType {
</strong>  EMPTY,
  GTE,
  EQ,
}
</code></pre>

Here is an example of a customized `ClaimRequest` that, [when requested](client.md#request), will trigger the generation of a proof from the latest data of the groupId `0x42c768bb8ae79e4c5c05d3b51a4ec74a` . The user should have exactly a value of 2 in this group.

```typescript
import { ClaimRequest } from "@sismo-core/sismo-connect-client";

const CLAIM: ClaimRequest = { 
    // ID of the group the user should be a member of
    groupId: "0x42c768bb8ae79e4c5c05d3b51a4ec74a",
    // timestamp of the data used for the group
    // default: latest
    groupTimestamp: "latest",
    // value the user should have in the group
    // default: 1
    value: 2, 
    // comparator for the requested value
    // default: "GTE" -> "greater than or equal"
    // here the user value in the group should be exactly 2
    claimType: ClaimType.EQ,
    isOptional: false;
    isSelectableByUser: true;
};
sismoConnect.request({ claim: CLAIM });
```

{% hint style="info" %}
More info on groups and values [here](../../knowledge-base/resources/technical-concepts/data-groups.md#verifiable-claims-and-data-groups).
{% endhint %}

### `SignatureRequest`

The `SignatureRequest` contains a message that the user will embed in a proof. In this way, we can think of this message as a signature.

```typescript
import { SignatureRequest } from "@sismo-core/sismo-connect-client";

const SIGNATURE: SignatureRequest = { 
    message: "Your message here"
};

sismoConnect.request({ signature: SIGNATURE });
```

### `request()`

```typescript
function request({ 
    claims, 
    auths, 
    // OR
    claim,
    auth,
    
    signature, 
    namespace, 
    callbackUrl,
    callbackPath, // deprecated
}: RequestParams): void
```

The `request` function redirects your user to the Sismo Vault App to generate the proof.

```typescript
export type RequestParams = {
  claims?: ClaimRequest[];
  auths?: AuthRequest[];
  // If you have only one claim or one auth
  claim?: ClaimRequest;
  auth?: AuthRequest;
  
  signature?: SignatureRequest;
  namespace?: string;
  // the url to redirect your users with proofs
  // default: empty
  // example: /custom-callback-url
  callbackUrl?: string;
  callbackPath?: string; // deprecated
};
```

**Here is an example of a customized usage of the request function:**

You want to create an NFT Airdrop for **holders of a Nouns DAO NFT** owning a **Gitcoin Passport** and a **Twitter account**.

So you want your users to prove that they own a **Nouns DAO NFT**. But in order to make your airdrop more Sybil resistant you will require them to also prove a **Gitcoin Passport ownership with  a passport value greater or equal to 15** and a **Twitter account ownership**.&#x20;

These proofs should be made for the service named **"sismo-edition".** When the proofs are generated, you want your users to be redirected to the claim page of your website at the path **"https://my-nft-drop.xyz/sismo-edition/claim-nft"**.

You will then use these proofs to airdrop a NFT if they are valid.

<pre class="language-typescript"><code class="lang-typescript">import { 
  AuthRequest, 
  AuthType, 
  ClaimRequest, 
  ClaimType,
  SismoConnect
} from "@sismo-core/sismo-connect-client";

// create a client config with an appId
const sismoConnect = SismoConnect({
  appId: '0xf4977993e52606cfd67b7a1cde717069',
})

// auth request for a proof of Twitter account ownership
const twitterRequest: AuthRequest = { 
    authType: AuthType.TWITTER,
};

// claim request for a proof of "Nouns DAO Nft holders" group membership
const nounsDaoRequest: ClaimRequest = { 
    // id of the group nouns-dao-nft-holders
    // https://factory.sismo.io/groups-explorer?search=nouns-dao-nft-holders
    groupId: "0x311ece950f9ec55757eb95f3182ae5e2",
};

// claim request for a proof of "Gitcoin Passport holders" group membership
const gitcoinPassportRequest: ClaimRequest = { 
    // id of the group gitcoin-passport-holders
    // https://factory.sismo.io/groups-explorer?search=gitcoin-passport-holders
    groupId: "0x1cde61966decb8600dfd0749bd371f12",
    // users should have at least 15 as value in the group to claim the airdrop
    value: 15,
    claimType: GTE
};

// redirect users to the Vault App to generate proofs based on the requirements
// expressed in the auth and claim requests
sismoConnect.request({
<strong>    auth: twitterRequest,
</strong>    claims: [nounsDaoRequest, gitcoinPassportRequest],
    namespace: "sismo-edition",
    callbackUrl: "https://my-nft-drop.xyz/sismo-edition/claim-nft"
})
</code></pre>

{% hint style="info" %}
Namespace is highly interesting when you want your users to generate proofs for different services in an app. You can see more information about how to use it in the [Sismo Connect Server package documentation](server.md).&#x20;
{% endhint %}

### `getResponse()`

```typescript
function getResponse(): SismoConnectResponse | null
```

The `getResponse` function returns the [`SismoConnectResponse`](client.md#sismoconnectresponse) object containing the proofs generated by your user after coming back from Sismo Vault App. Proofs can be sent to your backend and verified thanks to the [`@sismo-core/sismo-connect-server`](https://github.com/sismo-core/sismo-connect-packages/tree/main/packages/sismo-connect-server) package OR sent to your contract that uses the [Sismo Connect Solidity library](solidity.md) to verify.

#### `getResponseBytes()`

```typescript
function getResponseBytes(): string | null
```

The `getResponseBytes` function returns the response encoded in bytes usable by the [`@sismo-core/sismo-connect-solidity`](https://github.com/sismo-core/sismo-connect-packages/tree/main/packages/sismo-connect-solidity) Librairy.

#### `SismoConnectResponse`

```typescript
type SismoConnectResponse = {
    // the appId registered in the Factory
    appId: string;
    // Sismo Connect version
    version: string;
    // service from which the proof is requested
    namespace?: string;
    signedMessage?: string;
    proofs: SismoConnectProof[]
}

type SismoConnectProof = {
    auths?: Auth[];
    claims?: Claim[];
    provingScheme: string;
    proofData: string;
    extraData: any;
}
```
