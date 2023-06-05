---
description: Verify proofs from your users
---

# Sismo Connect Server: Verify Offchain

The Sismo Connect Server is a backend package built on top of the [Hydra-S2 Verifier](../../knowledge-base/resources/technical-concepts/proving-schemes/hydra-s2.md) to easily verify proofs from your users off-chain.&#x20;

<figure><img src="../../.gitbook/assets/Sismo Connect offchain Flow.png" alt=""><figcaption><p>Sismo Connect offchain Flow</p></figcaption></figure>

## Installation

Install [Sismo Connect](../../welcome-to-sismo/what-is-sismo-connect.md) server package in your backend with `npm` or `yarn`:

```bash
# with npm
npm i @sismo-core/sismo-connect-server
# with yarn
yarn add @sismo-core/sismo-connect-server
```

{% hint style="success" %}
Make sure to have at least v18.15.0 as Node version. You can encounter issues with ethers dependencies if not.
{% endhint %}

## Usage

### Configuration

The first step for integrating Sismo Connect in your backend is to create a `sismoConnectServerConfig`. This config will require an `appId` and can be customized with optional fields. You can go to the [Sismo Factory](https://factory.sismo.io/apps-explorer) to register an appId ([here is a tutorial](../../sismo-factory/create-a-sismo-connect-app.md)).

This `config` is then used to create a `SismoConnect` instance that will be used to verify proofs.

<pre class="language-typescript"><code class="lang-typescript"><strong>import { SismoConnect, SismoConnectServerConfig } from "@sismo-core/sismo-connect-server";
</strong>
const config: SismoConnectServerConfig = {
  // you will need to register an appId in the Factory
  appId: "0x8f347ca31790557391cec39b06f02dc2",
}

// create a new Sismo Connect instance with the server configuration
const sismoConnect = SismoConnect(config);
</code></pre>

### Verify proofs from your users

Proofs need to be sent from your frontend to your backend with a `SismoConnectResponse`. The `verify` function takes as inputs the `SismoConnectResponse` but also requests to verify that proofs held in the response are cryptographically valid with respect to these requests.

```typescript
import { SismoConnectVerifiedResult, AuthType } from "@sismo-core/sismo-connect-server";

async function verifyResponse(sismoConnectResponse: SismoConnectResponse) {
  // verifies the proofs contained in the sismoConnectResponse
  // with respect to the different auths
  // and the group(s) in the claim(s)
  // i.e. user prove they own a Vault, a Twitter account
  // and they are member of the group with id "0x42c768bb8ae79e4c5c05d3b51a4ec74a"
  const result: SismoConnectVerifiedResult = await sismoConnect.verify(
    sismoConnectResponse,
    {
      // proofs in the sismoConnectResponse should be valid
      // with respect to a Vault and Twitter account ownership
      auths: [
        { authType: AuthType.VAULT }, 
        { authType: AuthType.TWITTER }
      ],
      // proofs in the sismoConnectResponse should be valid
      // with respect to a specific group membership
      // here the group with id 0x42c768bb8ae79e4c5c05d3b51a4ec74a
      claims: [{ groupId: "0x42c768bb8ae79e4c5c05d3b51a4ec74a"}],
    }
  )

  // vaultId = hash(userVaultSecret, appId).
  // the vaultId is an app-specific, anonymous identifier of a vault
  const vaultId = result.getUserId(AuthType.VAULT)
  // you can also get the twitterId of the user
  const twitterId = result.getUserId(AuthType.TWITTER)
}
```

## Documentation

### **`SismoConnectServerConfig`**

The `SismoConnectServerConfig` allows you to fully customize your Sismo Connect integration in your backend. Its only mandatory field is the `appId`. For more liberty when prototyping, it also comes with an optional devMode field that allows developers to only verify that proofs are cryptographically valid without checking the requests made.

```typescript
export type SismoConnectServerConfig = {
  // the appId registered in the Factory
  appId: string,
  devMode?: {
    // will use the Dev Sismo Data Vault https://dev.vault-beta.sismo.io/
    enabled?: boolean, 
  },
  options?: {
    // optional ethers provider you can use to check the Merkle Tree root for groups
    // default: public gnosis provider
    provider?: Provider;
    // additional options for the verifier
    verifier?: VerifierOpts;
  }
};

export type VerifierOpts = {
  // additonal options for the hydras2 verifier
  hydraS2?: HydraS2VerifierOpts;
  isDevMode?: boolean;
};

export type HydraS2VerifierOpts = {
  // the provider referenced in the SismoConnectServerConfig
  provider?: Provider;
  // the commitment mapper registry contract address used by the verifier
  // default: https://gnosisscan.io/address/0xe205Fb31B656791850AC65f0623937Bf6170a5Da
  commitmentMapperRegistryAddress?: string;
  // the available roots registry contract address used by the verifier
  // default: https://gnosisscan.io/address/0x453bF14103CC941A96721de9A32d5E3d3095e049
  availableRootsRegistryAddress?: string;
  isDevMode?: boolean;
};
```

### `SismoConnectResponse`

The SismoConnectResponse needs to be sent by the frontend so that the backend can verify the proofs contained in it.

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

export type SismoConnectProof = {
  auths?: Auth[];
  claims?: Claim[];
  provingScheme: string;
  proofData: string;
  extraData: any;
};
```

### `verify()`

```typescript
type SismoConnectVerifiedResult = {
  public auths: VerifiedAuth[];
  public claims: VerifiedClaim[];
  public signedMessage: string | undefined;
  public response: SismoConnectResponse;
}

export type VerifiedClaim = Claim & {
  // the identifier of the proof, unique for each different namespace
  proofId: string;
  proofData: string;
}

export type Claim = {
  claimType?: ClaimType;
  groupId?: string;
  groupTimestamp?: number | "latest";
  isSelectableByUser?: boolean;
  value?: number;
  extraData?: any;
}

export type VerifiedAuth = Auth & {
  proofData: string;
}

export type Auth = {
  authType: AuthType;
  isAnon?: boolean;
  isSelectableByUser?: boolean;
  // Contains the account id of the user regarding the requested auth
  // May contain a vaultId, a githubId or a twitterId
  userId?: string;
  extraData?: any;
}

async function verify(
  sismoConnectResponse: SismoConnectResponse,
  {
    claims: ClaimRequest[],
    auths: AuthRequest[],
    signature: SignatureRequest,
    namespace: string,
  }
): Promise<SismoConnectVerifiedResult>;
```

If the proofs contained in the `sismoConnectResponse` are valid, the function should return a `SismoConnectVerifiedResult`, otherwise, it should throw an error.

You can easily extract `userIds` and `proofIds` from this `SismoConnectVerifiedResult` , these are ids that can be leveraged in your application to create anonymous databases (with `vaultId`, a specific kind of `userId` that is the hash of the `appId` and the `userVaultSecret`) and prevent double spendings in your application (submitting two times the same cryptographic proof).

{% hint style="success" %}
If you want to learn more about `vaultId` and `proofId`, check the [**Vault & Proof Identifiers section**](../../knowledge-base/resources/technical-concepts/vault-and-proof-identifiers.md).
{% endhint %}

Here are some small snippets to understand how you can use `vaultIds` and `proofIds` to your advantage when building you application. While the `vaultId` will always be the same for any proof generated in the same vault, `proofIds` will change depending on the `appId`, the `groupId`, the `groupTimestamp` and the `namespace` that were used to generate the proof.

You can see below an example with proofs generated for different namespaces in the same app and using the same exact group.

#### Frontend

<pre class="language-typescript"><code class="lang-typescript">// private-poll-1.tsx
import { SismoConnect, SismoConnectClientConfig, AuthType, ClaimRequest, AuthRequest } from "@sismo-core/sismo-connect-client";

const sismoConnect = SismoConnect({
  // sismoConnectClientConfig will only appId
  appId: "0x8f347ca31790557391cec39b06f02dc2", 
});

const CLAIM: ClaimRequest = { 
    groupId: "0x42c768bb8ae79e4c5c05d3b51a4ec74a",
};

const AUTH: AuthRequest = { 
    authType: AuthType.VAULT,
};

<strong>sismoConnect.request({ 
</strong>  claims: [CLAIM],
  auths: [AUTH],
  namespace: "my-private-poll-1", // 👈 --> namespace for private poll 1
});

const responseOne = sismoConnect.getResponse();

// you then send this sismoConnectResponse to your backend
</code></pre>

<pre class="language-typescript"><code class="lang-typescript"><strong>// you do the same with another poll 
</strong>// private-poll-2.tsx
import { SismoConnect, SismoConnectClientConfig, AuthType, ClaimRequest, AuthRequest } from "@sismo-core/sismo-connect-client";

const sismoConnect = SismoConnect({
  appId: "0x8f347ca31790557391cec39b06f02dc2", 
});

const CLAIM: ClaimRequest = { 
    groupId: "0x42c768bb8ae79e4c5c05d3b51a4ec74a",
};

const AUTH: AuthRequest = { 
    authType: AuthType.VAULT,
};

sismoConnect.request({ 
  claims: [CLAIM],
  auths: [AUTH],
  namespace: "my-private-poll-2", // 👈 --> namespace for private poll 2
});

const responseTwo = sismoConnect.getResponse();

// you then send this sismoConnectResponse to your backend
</code></pre>

#### Backend

<pre class="language-typescript"><code class="lang-typescript">import { SismoConnectVerifiedResult, SismoConnect, AuthType, ClaimRequest, AuthRequest } from "@sismo-core/sismo-connect-server";

<strong>const sismoConnect = SismoConnect({
</strong>  appId: "0x8f347ca31790557391cec39b06f02dc2", 
});

const CLAIM: ClaimRequest = { 
    groupId: "0x42c768bb8ae79e4c5c05d3b51a4ec74a",
};

const AUTH: AuthRequest = { 
    authType: AuthType.VAULT,
};

const resultOne: SismoConnectVerifiedResult = await sismoConnect.verify(
    responseOne, // 👈 --> sismoConnectResponse for private poll 1
    {
        claims: [CLAIM],
        auths: [AUTH],
    }
);


const resultTwo: SismoConnectVerifiedResult = await sismoConnect.verify(
    reponseTwo, // 👈 --> sismoConnectResponse for private poll 2
    {
        claims: [CLAIM],
        auths: [AUTH],
    }
);


const proofIdOne = resultOne.claims[0].proofId;
// vaultId = hash(userVaultSecret, appId). 
// the vaultId is an app-specific, anonymous identifier of a vault
const vaultIdOne = resultOne.getUserId(AuthType.VAULT);

const proofIdTwo = resultTwo.claims[0].proofId;
const vaultIdTwo = resultOne.getUserId(AuthType.VAULT);

// vaultIdOne === vaultIdTwo
// but
// proofIdOne !== proofIdTwo
// You can know if an vaultId has already voted on a private poll 
// by storing it 🤘
</code></pre>
