---
description: Verify proofs from your users
---

# zkConnect Server: Verify Off-chain

The [zkConnect](../../what-is-sismo/zkconnect.md) Server is a backend package built on top of the [Hydra-S2 Verifier](https://github.com/sismo-core/hydra-s2-zkps) to easily verify proofs from your users off-chain. You can see a full guide on how to integrate zkConnect into your application [here](../../tutorials/zkconnect/zk-connect-guide.md).

## Installation

Install [zkConnect](../../what-is-sismo/zkconnect.md) server package in your backend with `npm` or `yarn`:

```bash
# with npm
npm i @sismo-core/zk-connect-server
# with yarn
yarn add @sismo-core/zk-connect-server
```

{% hint style="success" %}
Make sure to have at least v18.15.0 as Node version. You can encounter issues with ethers dependencies if not.
{% endhint %}

## Usage

#### Configuration

The first step for integrating zkConnect in your backend is to create a `zkConnectConfig`. This config will require an `appId` and can be customized with optional fields. You can go to the [Sismo Factory](https://factory.sismo.io/apps-explorer) to register an appId.

```typescript
import { ZkConnect, ZkConnectServerConfig, DataRequest } from "@sismo-core/zk-connect-server";

const zkConnectConfig: ZkConnectServerConfig = {
  // you will need to register an appId in the Factory
  appId: "0x8f347ca31790557391cec39b06f02dc2",
}

// create a new ZkConnect instance with the server configuration
const zkConnect = ZkConnect(zkConnectConfig);
```

#### Create a DataRequest

The dataRequest will be used to verify that the proof is valid with respect to a particular groupId.

```typescript
const DATA_REQUEST = DataRequest({ groupId: "0x42c768bb8ae79e4c5c05d3b51a4ec74a"});
```

#### Verify proofs from your users

Proofs need to be sent to your backend with a ZkConnectResponse. The verify function takes as inputs a ZkConnectResponse and a DataRequest and verifies that the proof is cryptographically valid with respect to the groupId of the DataRequest.

```typescript
// verifies the proofs contained in the zkConnectResponse 
// with respect to the group in the dataRequest
const { vaultId } = await zkConnect.verify(
  zkConnectResponse,
  { dataRequest: DATA_REQUEST }
);
```

## Documentation

#### **`ZkConnectServerConfig`**

The `ZkConnectServerConfig` allows you to fully customize your zkConnect integration in your backend. Its only mandatory field is the `appId`. For more liberty when prototyping, it also comes with an optional devMode field that allows developers to only verify that proofs are cryptographically valid without respect to a certain group.

```typescript
export type ZkConnectServerConfig = {
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
  // the provider referenced in the ZKConnectServerConfig
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

#### `ZkConnectResponse`

The ZkConnectResponse needs to be sent by the frontend so that the backend can verify the proofs contained in it. You can in the types that the ZkConnectResponse can contain an `authProof`, that is a proof of Data Vault ownership, or `verifiableStatements` that can contain proofs about [Data Shards](../../what-is-sismo/data-vault.md).&#x20;

{% hint style="info" %}
The `authProof` will only be provided if `verifableStatements` are empty, i.e when no `DataRequests` where made by the frontend.
{% endhint %}

```typescript
type ZkConnectRequest = {
    // the appId registered in the Factory
    appId: string;
    // the dataRequest with the statementRequests
    dataRequest?: DataRequestType;
    // service from which the proof is requested
    namespace?: string;
    // the path to redirect your users with proofs
    callbackPath?: string;
    // zk connect version
    version: string;
};

type ZkConnectResponse = Omit<ZkConnectRequest, "callbackPath" | "dataRequest"> & {
    // proof of a Data Vault ownership
    authProof?: AuthProof;
    // statements containing proofs to verify
    verifiableStatements: VerifiableStatement[];
};
```

#### `verify`

```typescript
export type ZkConnectVerifiedResult = ZkConnectResponse & {
  // id of the user Data Vault for your appId
  vaultId: string;
  // statements containing proofs that were verified
  verifiedStatements: VerifiedStatement[];
};

export type VerifiedStatement = VerifiableStatement & { 
  // id of the verified proof
  proofId: string 
};

async function verify(zkConnectResponse: ZkConnectResponse,{ dataRequest, namespace }: VerifyParamsZkConnect): Promise<ZkConnectVerifiedResult>;
```

If the proof contained in the `zkConnectResponse` is valid, the function should return a `ZkConnectVerifiedResult`, otherwise, it should return an error.

In a `ZkConnectVerifiedResult`, you can get a [`vaultId`](../../technical-concepts/vault-and-proof-identifiers.md), which is a unique identifier that identifies a [Data Vault](../../what-is-sismo/data-vault.md) from Sismo. You can also choose to get `proofIds` in `verifiedStatements`.

```typescript
// get a vaultId out of valid proofs
const { vaultId } = await zkConnect.verify(
  zkConnectResponse,
  { dataRequest: DATA_REQUEST }
);

// get vaultId and verifiedStatements out of valid proofs
const { vaultId, verifiedStatements } = await zkConnect.verify(
  zkConnectResponse,
  { dataRequest: DATA_REQUEST }
);

// get a proofId out of a verifiedStatement
const firstProofId = verifiedStatements[0].proofId;

```

#### VaultId and Proof Ids

It is worth noting that the `vaultId` is deterministically generated based on the user vault secret and the `appId` with a Poseidon hash. This means that if a user has already used your app, the `vaultId` will be the same each time this user use your app but it will be different for other apps. This is useful if you want to store the `vaultId` in your database to link it to a user while preserving the privacy of this same user between apps. You can learn more about the `vaultId` [here](../../technical-concepts/vault-and-proof-identifiers.md).

You can also find a `proofId` in the `verifiedStatements` of the `ZkConnectVerifiedResult`. This `proofId` is a unique identifier that identifies a proof from Sismo. This `proofId` is also deterministically generated based on the `appId`, the `namespace`, the `groupId` and the `groupTimestamp`. This `proofId` will allow you to know if a user already proved something to your app for a specific namespace and group.&#x20;

This `proofId` can be useful if you want to allow your users to participate in private polls while ensuring only one vote each time. For each poll, you can specify a different namespace such as `namespace: "my-poll-x"` so that a different proofId is computed each time. By storing the `proofId` alongside the `vaultId` in your apps, you can now ensure that a user with a specific `vaultId` only votes one time for each private polls.

Here are some small snippets to get&#x20;

#### Frontend

<pre class="language-typescript"><code class="lang-typescript">// private-poll-1.tsx
import { ZkConnect, ZkConnectClientConfig, DataRequest } from "@sismo-core/zk-connect-client";

const zkConnect = ZkConnect({
  // zkConnectClientConfig will only appId
  appId: "0x8f347ca31790557391cec39b06f02dc2", 
});

const DATA_REQUEST = DataRequest({ 
    groupId: "0x42c768bb8ae79e4c5c05d3b51a4ec74a",
});
<strong>zkConnect.request({ 
</strong>  dataRequest: DATA_REQUEST,
  namespace: "my-private-poll-1", // 👈 --> namespace for private poll 1
});

const zkConnectResponseOne = zkConnect.getResponse();

// you then send this zkConnectResponse to your backend
</code></pre>

```typescript
// you do the same with another poll 
// private-poll-2.tsx
import { ZkConnect, ZkConnectClientConfig, DataRequest } from "@sismo-core/zk-connect-client";

const zkConnect = ZkConnect({
  appId: "0x8f347ca31790557391cec39b06f02dc2", 
});

const DATA_REQUEST = DataRequest({ 
    groupId: "0x42c768bb8ae79e4c5c05d3b51a4ec74a",
});
zkConnect.request({ 
  dataRequest: DATA_REQUEST, 
  namespace: "my-private-poll-2", // 👈 --> namespace for private poll 2
});

const zkConnectResponseTwo = zkConnect.getResponse();

// you then send this zkConnectResponse to your backend
```

#### Backend

<pre class="language-typescript"><code class="lang-typescript">import { DataRequest, ZkConnect, ZkConnectServerConfig } from "@sismo-core/zk-connect-server";
<strong>
</strong><strong>const zkConnect = ZkConnect({
</strong>  appId: "0x8f347ca31790557391cec39b06f02dc2", 
});

const DATA_REQUEST = DataRequest({ 
    groupId: "0x42c768bb8ae79e4c5c05d3b51a4ec74a",
});


const { vaultIdOne, verifiedStatementsOne } = await zkConnect.verify(
    zkConnectResponseOne, // 👈 --> zkConnectResponse for private poll 1
    {
      dataRequest: DATA_REQUEST,
    }
);

const { vaultIdTwo, verifiedStatementsTwo } = await zkConnect.verify(
    zkConnectResponseOne, // 👈 --> zkConnectResponse for private poll 2
    {
      dataRequest: DATA_REQUEST,
    }
);

const proofIdOne = verifiedStatementsOne[0].proofId;
const proofIdTwo = verifiedStatementsTwo[0].proofId;

// vaultIdOne === vaultIdTwo
// but
// proofIdOne !== proofIdTwo
// You can know if a vaultId has already voted on a private poll 
// by storing the proofIds 🤘
</code></pre>
