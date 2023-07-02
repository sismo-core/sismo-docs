# Signature

A `Signature` is a specific message embedded in a generated proof that will be checked when verifying the proof.  It can be requested to the Data Vault thanks to a `SignatureRequest` .

<details>

<summary>Why a signature is often needed when verifying proofs onchain?</summary>

The signed message is not mandatory when you interact with your contracts, but it is very often needed. As far as your users are generating valid proofs, it could be quite easy for a third party to front-run them by just taking their proof and making their own call to your smart contracts with it.

To overcome this issue, we offer a way to embed a specific message in a proof. This way it can be thought of as a signature since this proof could not be valid without checking successfully that the signed message is correct onchain.&#x20;

For example, it is mandatory to request the user to embed the address where they want to receive an airdrop in a proof. If a third party takes the proof, the call will be reverted with a signature mismatch message, effectively protecting your users from being front-run.

Example: You are a DAO voting platform, and you want to get the vote of a user onchain. To do so, the user will create a signature containing a message (his vote) that will be used to generate the proof. When verifying the proof onchain, you are also verifying that the proof contains the message. It is then impossible for a malicious actor to take your proof and vote for something else.

</details>

## Type definitions&#x20;

The `SignatureRequest` is an object with the following properties:

* `message` (required): the message that the user is going to sign.
* `isSelectableByUser`(optional): by default set to `false`. Allows the user to edit the message in the Sismo Data Vault app. &#x20;

```typescript
// Types (typescript version)
type SignatureRequest = {
    message: string;
    isSelectableByUser?: boolean;
};
```

## Integrations

`SignatureRequest`  is made in the front end using either the [`sismo-connect-react` package](packages/react.md) or the [`sismo-connect-client` package](packages/client.md).

The proof with the requested signature is then verified either in a backend using the [`sismo-connect-server` package](packages/server.md) or in a smart contract using the [`sismo-connect-solidity` package](packages/solidity.md).

{% hint style="success" %}
If you are verifying your proofs in a smart contract, you will need to encode your singature with some ABI encoder like [Viem](https://viem.sh/docs/abi/encodeAbiParameters.html) or [EthersJS](https://docs.ethers.org/v5/api/utils/abi/coder/).
{% endhint %}

### Making a SignatureRequest - Front-end integration

{% hint style="info" %}
Making an `SignatureRequest` is only possible if it is made alongside an auth or a claim request.&#x20;
{% endhint %}

{% tabs %}
{% tab title="React Component" %}
The `SismoConnectButton` React component is available from the [sismo-connect-react package](packages/react.md).  It is a wrapper of the [sismo-connect-client package](packages/client.md).&#x20;

* SignatureRequest is passed to the `SismoConnectButton` through the `signature` props
* Responses are received through either:
  1. &#x20;the `onResponse: (response: SismoConnectResponse) => void` callback for offchain verification or,
  2. &#x20;the `onResponseBytes (response: string) => void` callback for onchain verification.

#### SignatureRequest - code example

{% code title="app.tsx" overflow="wrap" %}
```tsx
import { SismoConnectButton, SismoConnectClientConfig, SismoConnectResponse, AuthType } from "@sismo-core/sismo-connect-react";

<SismoConnectButton 
    // the client config created
    config={config}
    // signature request must be made with at least an auth or one claim
    auth={{authType: AuthType.VAULT}}
    // your signature request
    // that needs to be encoded if you verify proofs in smart contracts
    signature={{message: "0x00"}}
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
The `useSismoConnect` hook is available from the [sismo-connect-react package](packages/react.md).  It is a wrapper of the [sismo-connect-client package](packages/client.md).  The `useSismoConnect` hook  exposes the `sismoConnect` variable.&#x20;

* One or multiple claim requests can be made using the `sismoConnect.request()` method available on the `sismoConnect` variable.
* Responses could be received through either:
  1. &#x20;the `sismoConnect.getReponse()` method for offchain verification or,
  2. &#x20;the `sismoConnect.getResponseBytes()` method for onchain verification.

#### One ClaimRequest - code example

{% code title="App.tsx" overflow="wrap" %}
```tsx
import { useSismoConnect, SismoConnectResponse, AuthType } from "@sismo-core/sismo-connect-react";

const { sismoConnect } = useSismoConnect({ config });

function onClick(){
  sismoConnect.request({
      // signature request must be made with at least an auth or one claim
      auth: {authType: AuthType.VAULT};
      // your signature request
      // that needs to be encoded if you verify proofs in smart contracts
      signature: {message: "0x00"}
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
  1. &#x20;the `sismoConnect.getReponse()` method for offchain verification or,
  2. &#x20;the `sismoConnect.getResponseBytes()` method for onchain verification.

#### One ClaimRequest - code example

{% code title="app.ts" overflow="wrap" %}
```typescript
import { SismoConnect, SismoConnectResponse, AuthType } from "@sismo-core/sismo-connect-client";

const sismoConnect = SismoConnect({config});

// your signature request
function onClick(){
  sismoConnect.request({
      // signature request must be made with at least an auth or one claim
      auth: {authType: AuthType.VAULT};
      // your signature request
      // that needs to be encoded if you verify proofs in smart contracts
      signature: {message: "0x00"}
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

### Verifying a SignatureRequest

{% hint style="info" %}
Once a user has generated the proof on the Sismo Data Vault App, your application must verify it. This can be made either offchain in you backend or onchain in your smart contract. The [`sismo-connect-server` package](packages/server.md) exposes a `SismoConnect` variable.
{% endhint %}

{% tabs %}
{% tab title="Offchain verification - Node.js" %}
A signature request can be verified offchain on a backend server using the `sismoConnect.verify()` method available on a `SismoConnect` instance.

If the proof is valid `sismoConnect.verify()` returns a `result` of type `SismoConnectVerifiedResult` else it will throw an error.

#### SignatureRequest - code example

{% code overflow="wrap" %}
```typescript
import { SismoConnect, SismoConnectVerifiedResult, AuthType } from "@sismo-core/sismo-connect-server";

const sismoConnect = SismoConnect({config});

async function verifyResponse(sismoConnectResponse: SismoConnectResponse) {
  // verifies the proofs contained in the sismoConnectResponse
  const result: SismoConnectVerifiedResult = await sismoConnect.verify(
    sismoConnectResponse,
    {
      // signature request must be made with at least an auth or one claim
      auth: { authType: AuthType.VAULT },
      // your signature request
      signature: {message: "0x00"}
    }
  )
}
```
{% endcode %}
{% endtab %}

{% tab title="Onchain verification - Solidity" %}
The [`sismo-connect-solidity` package](packages/solidity.md) exposes a Sismo Connect Library which can be inherited by your contract either using Foundry or Hardhat.&#x20;

The Sismo Connect Library exposes:&#x20;

* &#x20;a `verify()` function which allows you to verify a proof generated by the [Sismo Vault app](broken-reference) with respect to some requests directly in your contract. The verify() function takes an object containing:&#x20;
  1. a `responseBytes` send from the front end,
  2. a signature corresponding to the signature request made in the front end,
  3. an auth or a claim.&#x20;

#### SignatureRequest - code example

```solidity
contract MyContract is SismoConnect { // inherits from Sismo Connect library
 
// call SismoConnect constructor with your appId
constructor(bytes16 appId) SismoConnect(buildConfig(appId)) {}

    function doSomethingUsingSismoConnect(bytes memory sismoConnectResponse) public {    
        SismoConnectVerifiedResult memory result = verify({
            responseBytes: sismoConnectResponse,
            // signature request must be made with at least an auth or one claim
            auth: buildAuth({authType: AuthType.VAULT}),       
            // we want to check if the signed message provided in the response is the signature of the user's address
            signature: buildSignature({message: abi.encode(msg.sender)})      
        });
        
        // do something once verified
    }
}
```
{% endtab %}
{% endtabs %}
