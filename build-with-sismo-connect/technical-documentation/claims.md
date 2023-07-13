# Claims

Sismo Connect can be used to request zero-knowledge proofs (ZKPs) that attest group membership from users.

## Type definitions

The `ClaimRequest` holds all the information needed to generate such a proof. It has the following properties:

* `groupId` (required): the group identifier that the user must prove membership of in order to generate the proof. The groupId can be found in the [Sismo Factory](https://factory.sismo.io/groups-explorer).
* `value` (optional): by default set to “1”. Querying a specific value restricts eligibility to users belonging to the group with the minimum specified value or matching the exact value.
* `claimType` (optional): by default `ClaimType.GTE`. Defines the type of group membership required. The following claim types are currently supported:
  * `ClaimType.GTE`: the user must prove that they own an account with at least the requested value.
  * `ClaimType.EQ`: the user must prove that he owns an account with the exact requested value.
* `groupTimestamp` (optional): set to “latest” by default. Groups are composed of snapshots generated either once, daily, or weekly. Each Group Snapshot generated has a timestamp associated with them. By default, the selected group is the latest Group Snapshot generated. But you are free to select a Group Snapshot with a different timestamp than the latest generated one.
* `isOptional`(optional): by default set to `false`. If set to `true` the user can optionally prove their group membership.
* `isSelectableByUser`(optional): by default set to `false` and only available for `ClaimType.GTE`. This allows users to selectively choose the value used to generate the proof.

```typescript
// Types (typescript version)
enum ClaimType {
    GTE = 0,
    GT = 1, // not supported
    EQ = 2,
    LT = 3, // not supported
    LTE = 4 // not supported
}

type ClaimRequest = {
    groupId: string;
    claimType?: ClaimType;
    groupTimestamp?: number | "latest";
    value?: number;
    isOptional?: boolean;
    isSelectableByUser?: boolean;
};
```

## Integrations

Claim requests are made in the front end using either the [`sismo-connect-react` package](packages/react.md) or the [`sismo-connect-client` package](packages/client.md).

Requests are then verified either in a backend using the [`sismo-connect-server` package](packages/server.md) or in a smart contract using the [`sismo-connect-solidity` package](packages/solidity.md).

### Making a ClaimRequest - Front-end integration

{% hint style="info" %}
Making an `ClaimRequest` is only possible in the front end as the request redirects users to the Sismo Vault app, where users can generate a zero-knowledge proof.
{% endhint %}

{% tabs %}
{% tab title="React Component" %}
The `SismoConnectButton` React component is available from the [sismo-connect-react package](packages/react.md). It is a wrapper of the [sismo-connect-client package](packages/client.md).

* ClaimRequests are passed as props to the `SismoConnectButton` either through:
  1. the `claim` props for one claim request: `ClaimRequest` or,
  2. the `claims` props for several claim requests: `ClaimRequest[]`
* Responses are received through either:
  1. the `onResponse: (response: SismoConnectResponse) => void` callback for offchain verification or,
  2. the `onResponseBytes (response: string) => void` callback for onchain verification.

**One ClaimRequest - code example**

{% code title="app.tsx" overflow="wrap" %}
```tsx
import { SismoConnectButton, SismoConnectClientConfig, SismoConnectResponse } from "@sismo-core/sismo-connect-react";

<SismoConnectButton 
    // the client config created
    config={config}
    // request a proof of group membership 
    // (here the group with id 0x42c768bb8ae79e4c5c05d3b51a4ec74a)
    claim={{groupId: "0x42c768bb8ae79e4c5c05d3b51a4ec74a"}}
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
{% endcode %}

**Multiple AuthRequests - code example**

{% code title="app.tsx" overflow="wrap" %}
```tsx
// You can also create several auth requests 
// in the same button
import { SismoConnectButton, SismoConnectClientConfig, SismoConnectResponse } from "@sismo-core/sismo-connect-react";

<SismoConnectButton 
    config={config}
    // request multiple proofs of group membership 
    // (here the groups with id 0x42c768bb8ae79e4c5c05d3b51a4ec74a and         0x8b64c959a715c6b10aa8372100071ca7)
    claims={[
        {groupId: "0x42c768bb8ae79e4c5c05d3b51a4ec74a"},
        {groupId: "0x8b64c959a715c6b10aa8372100071ca7"},
    ]}
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
{% endcode %}
{% endtab %}

{% tab title="React Hook" %}
The `useSismoConnect` hook is available from the [sismo-connect-react package](packages/react.md). It is a wrapper of the [sismo-connect-client package](packages/client.md). The `useSismoConnect` hook exposes the `sismoConnect` variable.

* One or multiple claim requests can be made using the `sismoConnect.request()` method available on the `sismoConnect` variable.
* Responses could be received through either:
  1. the `sismoConnect.getReponse()` method for offchain verification or,
  2. the `sismoConnect.getResponseBytes()` method for onchain verification.

**One ClaimRequest - code example**

{% code title="App.tsx" overflow="wrap" %}
```tsx
import { useSismoConnect, SismoConnectResponse } from "@sismo-core/sismo-connect-react";

const { sismoConnect } = useSismoConnect({ config });

// request a proof of group membership 
// (here the group with id 0x42c768bb8ae79e4c5c05d3b51a4ec74a)
function onClick(){
  sismoConnect.request({
      claim: {groupId: "0x42c768bb8ae79e4c5c05d3b51a4ec74a"}
    });
}

// Proofs are available in two differents types depending on usage (offchain or onchaon verification)
const response: SismoConnectResponse | null = sismoConnect.getReponse();
const responseBytes: string | null  = sismoConnect.getResponseBytes();

if(response || responseBytes) {
// Send response to the backend to verify it or,
// Send responseBytes to smart contract to verify it. 
}

```
{% endcode %}

**Multiple ClaimRequests - code example**

{% code title="App.tsx" overflow="wrap" %}
```tsx
import { useSismoConnect, SismoConnectResponse } from "@sismo-core/sismo-connect-react";

const { sismoConnect } = useSismoConnect({ config });

// request multiple proofs of group membership 
// (here the groups with id 0x42c768bb8ae79e4c5c05d3b51a4ec74a and         0x8b64c959a715c6b10aa8372100071ca7)
function onClick(){
  sismoConnect.request({
      claims: [
      {groupId: "0x42c768bb8ae79e4c5c05d3b51a4ec74a"},
      {groupId: "0x8b64c959a715c6b10aa8372100071ca7"},
      ]
    });
}

// Proofs are available in two differents types depending on usage (offchain or onchaon verification)
const response: SismoConnectResponse | null = sismoConnect.getReponse();
const responseBytes: string | null  = sismoConnect.getResponseBytes();

if(response || responseBytes) {
// Send response to the backend to verify it or,
// Send responseBytes to smart contract to verify it. 
}
```
{% endcode %}
{% endtab %}

{% tab title="Typescript" %}
The [`sismo-connect-client` package](packages/client.md) exposes a `SismoConnect` variable.

* One or multiple ClaimRequests can be made using the `sismoConnect.request()` method available on a `SismoConnect` instance.
* Responses could be received through either:
  1. the `sismoConnect.getReponse()` method for offchain verification or,
  2. the `sismoConnect.getResponseBytes()` method for onchain verification.

**One ClaimRequest - code example**

{% code title="app.ts" overflow="wrap" %}
```typescript
import { SismoConnect, SismoConnectResponse } from "@sismo-core/sismo-connect-client";

const sismoConnect = SismoConnect({config});

// request a proof of group membership 
// (here the group with id 0x42c768bb8ae79e4c5c05d3b51a4ec74a)
function onClick(){
  sismoConnect.request({
      claim: {groupId: "0x42c768bb8ae79e4c5c05d3b51a4ec74a"}
    });
}

// Receive the proofs in two different formats
const response: SismoConnectResponse | null = sismoConnect.getReponse();
const responseBytes: string | null  = sismoConnect.getResponseBytes();

if(response || responseBytes) {
// Send response to the backend to verify it or,
// Send responseBytes to smart contract to verify it. 
}
```
{% endcode %}

**Multiple ClaimRequests - code example**

{% code title="app.ts" overflow="wrap" %}
```typescript
import { SismoConnect, SismoConnectResponse } from "@sismo-core/sismo-connect-client";

const sismoConnect = SismoConnect({config});

// request multiple proofs of group membership 
// (here the groups with id 0x42c768bb8ae79e4c5c05d3b51a4ec74a and         0x8b64c959a715c6b10aa8372100071ca7)
function onClick(){
  sismoConnect.request({
      claims: [
      {groupId: "0x42c768bb8ae79e4c5c05d3b51a4ec74a"},
      {groupId: "0x8b64c959a715c6b10aa8372100071ca7"},
      ] 
    });
}

// Receive the proofs in two different formats
const response: SismoConnectResponse | null = sismoConnect.getReponse();
const responseBytes: string | null  = sismoConnect.getResponseBytes();

if(response || responseBytes) {
// Send response to the backend to verify it or,
// Send responseBytes to smart contract to verify it. 
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Verifying a ClaimRequest

{% hint style="info" %}
Once a user has generated the proof on the Data Vault app, your application must verify it. This can be made either offchain in your backend or onchain in your smart contract. The [`sismo-connect-server` package](packages/server.md) exposes a `SismoConnect` variable.
{% endhint %}

{% tabs %}
{% tab title="Offchain verification - Node.js" %}
One or multiple claim requests can be verified offchain on a backend server using the `sismoConnect.verify()` method available on a `SismoConnect` instance.

If the proof is valid `sismoConnect.verify()` returns a `result` of type `SismoConnectVerifiedResult` else it will throw an error.

**One ClaimRequest - code example**

{% code overflow="wrap" %}
```typescript
import { SismoConnect, SismoConnectVerifiedResult } from "@sismo-core/sismo-connect-server";

const sismoConnect = SismoConnect({config});

async function verifyResponse(sismoConnectResponse: SismoConnectResponse) {
  // verifies the proofs contained in the sismoConnectResponse
  // with respect to the different claims
  const result: SismoConnectVerifiedResult = await sismoConnect.verify(
    sismoConnectResponse,
    {
      // proofs in the sismoConnectResponse should be valid
      // with respect to a group membership
      claim: {groupId: "0x42c768bb8ae79e4c5c05d3b51a4ec74a"}
    }
  )
}
```
{% endcode %}

**Multiple ClaimRequests - code example**

{% code overflow="wrap" %}
```typescript
import { SismoConnect, SismoConnectVerifiedResult } from "@sismo-core/sismo-connect-server";

const sismoConnect = SismoConnect({config});

async function verifyResponse(sismoConnectResponse: SismoConnectResponse) {
  // verifies the proofs contained in the sismoConnectResponse
  // with respect to the different claims
  const result: SismoConnectVerifiedResult = await sismoConnect.verify(
    sismoConnectResponse,
    {
      // proofs in the sismoConnectResponse should be valid
      // with respect to the groups memberships
      claims: [
        {groupId: "0x42c768bb8ae79e4c5c05d3b51a4ec74a"},
        {groupId: "0x8b64c959a715c6b10aa8372100071ca7"},
      ] 
    }
  )
}
```
{% endcode %}
{% endtab %}

{% tab title="Onchain verification - Solidity" %}
The [`sismo-connect-solidity` package](packages/solidity.md) exposes a Sismo Connect Library, which can be inherited by your contract either using Foundry or Hardhat.

The Sismo Connect Library exposes:

* a `verify()` function, which allows you to verify a proof generated by the [Sismo Vault app](broken-reference) with respect to some requests directly in your contract. The verify() function takes an object containing:
  1. a `responseBytes` send from the front end,
  2. an `claim` or `claims` corresponding to the claim request made in the front end.
* a `buildClaim()` helper to recreate the `ClaimRequest` made in the front end

**One ClaimRequest - code example**

```solidity
contract MyContract is SismoConnect { // inherits from Sismo Connect library
 
// call SismoConnect constructor with your appId
constructor(bytes16 appId) SismoConnect(buildConfig(appId)) {}

    function doSomethingUsingSismoConnect(bytes memory sismoConnectResponse) public {    
        SismoConnectVerifiedResult memory result = verify({
            responseBytes: sismoConnectResponse,
            claim: buildClaim({groupId: "0x42c768bb8ae79e4c5c05d3b51a4ec74a"}),       
        });
        
        // do something once verified
    }
}
```

**Multiple ClaimRequests - code example**

```solidity
contract MyContract is SismoConnect { // inherits from Sismo Connect library
 
// call SismoConnect constructor with your appId
constructor(bytes16 appId) SismoConnect(buildConfig(appId)) {}

    function doSomethingUsingSismoConnect(bytes memory sismoConnectResponse) public {    
        ClaimRequest[] memory claims = new ClaimRequest[](2);
        claims[0] = buildClaim({groupId: "0x42c768bb8ae79e4c5c05d3b51a4ec74a"})
        claims[1] = buildClaim({groupId: "0x8b64c959a715c6b10aa8372100071ca7"})
        
        SismoConnectVerifiedResult memory result = verify({
            responseBytes: sismoConnectResponse,
            claims: claims,       
        });

        // do something once verified
    }
}
```
{% endtab %}
{% endtabs %}
