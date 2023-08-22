# Auths

Sismo Connect can be used to authenticate a user from multiple sources, either from web2 or web3.

## Type definitions&#x20;

The `AuthRequest` is an object with the following properties:

* `AuthType` (required): defines the type of authentication required. The following authType are currently supported:&#x20;
  * `VAULT`: Sismo Connect returns the user's vaultId for the application that requested it. It is a deterministic anonymous identifier (hash(userVaultSecret, AppId, ..))\
    See more information about Vault Identifiers [here](../../data-vault/vault-and-proof-identifiers.md).
  * `GITHUB`: Sismo Connect returns the user's GitHub account id.&#x20;
  * `TWITTER` Sismo Connect returns the user's Twitter account id.&#x20;
  * `EVM_ACCOUNT`: Sismo Connect returns the user's Ethereum address.
  * `TELEGRAM` : Sismo Connect returns the user's Telegram account id.&#x20;
* `userId` (optional): requests the user to have a predefined account.
* `isOptional`(optional): by default set to `false`.  Allows the user to optionally authenticate with this `AuthType`.

```typescript
// Types (typescript version)
enum AuthType {
    VAULT = 0,
    GITHUB = 1,
    TWITTER = 2,
    EVM_ACCOUNT = 3,
    TELEGRAM = 4,
}

type AuthRequest = {
    authType: AuthType;
    userId?: string;
    isOptional?: boolean;
};
```

## Integrations

Authentication requests are made in the front end using either the [`sismo-connect-react` package](packages/react.md) or the [`sismo-connect-client` package](packages/client.md). Requests are then verified either in a back end using the [`sismo-connect-server` package](packages/server.md) or in a smart contract using the [`sismo-connect-solidity` package](packages/solidity.md).

### Making an AuthRequest - Front-end integration

{% hint style="info" %}
Making an `AuthRequest` is only possible in the front end as the request redirects users to the Sismo Data Vault app, where users can generate a zero-knowledge proof.
{% endhint %}

{% tabs %}
{% tab title="React Component" %}
The `SismoConnectButton` React component is available from the [sismo-connect-react package](packages/react.md).  It is a wrapper of the [sismo-connect-client package](packages/client.md).&#x20;

* AuthRequests are passed as props of the `SismoConnectButton` either through:
  1. &#x20;the `auth`  props for one authentication request: `AuthRequest` or,
  2. &#x20;the `auths` props for several authentication requests: `AuthRequest[]`
* Responses are received through either:
  1. &#x20;the `onResponse: (response: SismoConnectResponse) => void` callback for offchain verification or,
  2. &#x20;the `onResponseBytes (response: string) => void` callback for onchain verification.

#### One AuthRequest - code example

{% code title="app.tsx" overflow="wrap" %}
```tsx
import { SismoConnectButton, AuthType, SismoConnectClientConfig, SismoConnectResponse } from "@sismo-core/sismo-connect-react";

<SismoConnectButton 
    // the client config created
    config={config}
    // request a proof of account ownership 
    // (here Vault ownership)
    auth={{authType: AuthType.VAULT}}
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

#### Multiple AuthRequests - code example

{% code title="app.tsx" overflow="wrap" %}
```tsx
// You can also create several auth requests 
// in the same button
import { SismoConnectButton, AuthType, SismoConnectClientConfig, SismoConnectResponse } from "@sismo-core/sismo-connect-react";

<SismoConnectButton 
    config={config}
    // request multiple proofs of account ownership 
    // (here Vault ownership and Twitter account ownership)
    auths={[
        {authType: AuthType.VAULT},
        {authType: AuthType.TWITTER},
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
The `useSismoConnect` hook is available from the [sismo-connect-react package](packages/react.md).  It is a wrapper of the [sismo-connect-client package](packages/client.md).  The `useSismoConnect` hook exposes the `sismoConnect` variable.&#x20;

* One or multiple AuthRequests can be made using the `sismoConnect.request()` method available on the `sismoConnect` variable.
* The response could be received through either:
  1. &#x20;the `sismoConnect.getReponse()` method for offchain verification or,
  2. &#x20;the `sismoConnect.getResponseBytes()` method for onchain verification.

#### One AuthRequest - code example

{% code title="App.tsx" overflow="wrap" %}
```tsx
import { useSismoConnect, SismoConnectResponse, AuthType } from "@sismo-core/sismo-connect-react";

const { sismoConnect } = useSismoConnect({ config });

// Making the request a proof of account ownership 
// (here Vault ownership)
function onClick(){
  sismoConnect.request({
      auth: {authType: AuthType.VAULT}
    });
}

// Proofs are available in two differents types depending on usage (offchain or onchaon verification)
const response: SismoConnectResponse | null = sismoConnect.getResponse();
const responseBytes: string | null  = sismoConnect.getResponseBytes();

if(response || responseBytes) {
// Send response to the backend to verify it or,
// Send responseBytes to smart contract to verify it. 
}

```
{% endcode %}

#### Multiple AuthRequests - code example

{% code title="App.tsx" overflow="wrap" %}
```tsx
import { useSismoConnect, SismoConnectResponse, AuthType } from "@sismo-core/sismo-connect-react";

const { sismoConnect } = useSismoConnect({ config });

// Making the request for multiple proofs of account ownership 
// (here Vault ownership and Twitter account ownership)
function onClick(){
  sismoConnect.request({
      auths: [
      {authType: AuthType.VAULT},
      {authType: AuthType.TWITTER},
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

{% tab title="TypeScript" %}
The [`sismo-connect-client` package](packages/client.md) exposes a `SismoConnect` variable.

* One or multiple AuthRequests can be made using the `sismoConnect.request()` method available on a `SismoConnect` instance.
* The response could be received through either:
  1. &#x20;the `sismoConnect.getReponse()` method for offchain verification or,
  2. &#x20;the `sismoConnect.getResponseBytes()` method for onchain verification.

#### One AuthRequest - code example

{% code title="app.ts" overflow="wrap" %}
```typescript
import { SismoConnect, SismoConnectResponse, AuthType } from "@sismo-core/sismo-connect-client";

const sismoConnect = SismoConnect({config});

// Making the request a proof of account ownership 
// (here Vault ownership)
function onClick(){
  sismoConnect.request({
      auth: {authType: AuthType.VAULT}
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

#### Multiple AuthRequests - code example

{% code title="app.ts" overflow="wrap" %}
```typescript
import { SismoConnect, SismoConnectResponse, AuthType } from "@sismo-core/sismo-connect-client";

const sismoConnect = SismoConnect({config});

// Making the request for multiple proofs of account ownership 
// (here Vault ownership and Twitter account ownership)
function onClick(){
  sismoConnect.request({
      auths: [
      {authType: AuthType.VAULT},
      {authType: AuthType.TWITTER},
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

### Verifying an AuthRequest

Once a user has generated a ZKP on the Data Vault app, your application must verify it. This can be achieved in onchain smart contracts or offchain back ends.

{% tabs %}
{% tab title="offchain verification - Node.js" %}
The [`sismo-connect-server` package](packages/server.md) exposes a `SismoConnect` variable.

One or multiple AuthRequests can be verified offchain on a backend server using the `sismoConnect.verify()` method available on a `SismoConnect` instance.

* If the proof is valid `sismoConnect.verify()` returns a `result` of type `SismoConnectVerifiedResult` else it will throw an error,
* the `result.getUserId()` can be called as shown below to get the userId of the corresponding type.&#x20;

#### One AuthRequest - code example

{% code overflow="wrap" %}
```typescript
import { SismoConnect, SismoConnectVerifiedResult, AuthType } from "@sismo-core/sismo-connect-server";

const sismoConnect = SismoConnect({config});

async function verifyResponse(sismoConnectResponse: SismoConnectResponse) {
  // verifies the proofs contained in the sismoConnectResponse
  // with respect to the different auths
  // i.e. user prove they own a Vault
  const result: SismoConnectVerifiedResult = await sismoConnect.verify(
    sismoConnectResponse,
    {
      // proofs in the sismoConnectResponse should be valid
      // with respect to a Vault ownership
      auth: { authType: AuthType.VAULT },
    }
  )

  // vaultId = hash(userVaultSecret, appId).
  // the vaultId is an app-specific, anonymous identifier of a vault
  const vaultId = result.getUserId(AuthType.VAULT)
}
```
{% endcode %}

#### Multiple AuthRequests - code example

{% code overflow="wrap" %}
```typescript
import { SismoConnect, SismoConnectVerifiedResult, AuthType } from "@sismo-core/sismo-connect-server";

const sismoConnect = SismoConnect({config});

async function verifyResponse(sismoConnectResponse: SismoConnectResponse) {
  // verifies the proofs contained in the sismoConnectResponse
  // with respect to the different auths
  // i.e. user prove they own a Vault, a Twitter account
  const result: SismoConnectVerifiedResult = await sismoConnect.verify(
    sismoConnectResponse,
    {
      // proofs in the sismoConnectResponse should be valid
      // with respect to a Vault and Twitter account ownership
      auths: [
        { authType: AuthType.VAULT }, 
        { authType: AuthType.TWITTER }
      ],
    }
  )

  // vaultId = hash(userVaultSecret, appId).
  // the vaultId is an app-specific, anonymous identifier of a vault
  const vaultId = result.getUserId(AuthType.VAULT)
  // you can also get the twitterId of the user
  const twitterId = result.getUserId(AuthType.TWITTER)
}
```
{% endcode %}
{% endtab %}

{% tab title="onchain verification - Solidity" %}
The [`sismo-connect-solidity` package](packages/solidity.md) exposes a Sismo Connect Library, which can be inherited by your contract either using Foundry or Hardhat.&#x20;

The Sismo Connect Library exposes:&#x20;

* &#x20;a `verify()` function, which allows you to verify a proof generated by the [Sismo Vault app](broken-reference) with respect to some requests directly in your contract. The verify() function takes an object containing:&#x20;
  1. a `responseBytes` send from the front end,
  2. an `auth` or `auths` corresponding to the authentification request made in the front end.
* a `buildAuth()` helper to recreate the `AuthRequest` made in the front-end
* a `SismoConnectHelper.getUserId()` helper to retrieve the userId of the corresponding type once the proof has been verified.

#### One AuthRequest - code example

```solidity
contract MyContract is SismoConnect { // inherits from Sismo Connect library
 
// call SismoConnect constructor with your appId
constructor(bytes16 appId) SismoConnect(buildConfig(appId)) {}

    function doSomethingUsingSismoConnect(bytes memory sismoConnectResponse) public {    
        SismoConnectVerifiedResult memory result = verify({
            responseBytes: sismoConnectResponse,
            // we want users to prove that they own a Sismo Vault
            // we are recreating the auth request made in the frontend to be sure that 
            // the proof provided in the response is valid with respect to this auth request
            auth: buildAuth({authType: AuthType.VAULT}),       
        });
    
        // if the proofs is valid, we can take the userId from the verified result
        // in this case the userId is the vaultId (since we used AuthType.VAULT in the auth request) 
        // it is the anonymous identifier of a user's vault for a specific app 
        // --> vaultId = hash(userVaultSecret, appId)
        uint256 vaultId = SismoConnectHelper.getUserId(result, AuthType.VAULT);
        
        // do something with this vaultId for example
    }
}
```

#### Multiple AuthRequests - code example

```solidity
contract MyContract is SismoConnect { // inherits from Sismo Connect library
 
// call SismoConnect constructor with your appId
constructor(bytes16 appId) SismoConnect(buildConfig(appId)) {}

    function doSomethingUsingSismoConnect(bytes memory sismoConnectResponse) public {    
        // we want users to prove that they own a Sismo Vault and a Twitter Id
        // we are recreating the auth request made in the frontend to be sure that 
        // the proof provided in the response is valid with respect to this auth request
        
        AuthRequest[] memory auths = new AuthRequest[](2);
        auths[0] = buildAuth({authType: AuthType.VAULT})
        auths[1] = buildAuth({authType: AuthType.TWITTER})
        
        SismoConnectVerifiedResult memory result = verify({
            responseBytes: sismoConnectResponse,
            auths: auths,       
        });
        
        // --> vaultId = hash(userVaultSecret, appId)
        uint256 vaultId = SismoConnectHelper.getUserId(result, AuthType.VAULT);
        uint256 twitterId = SismoConnectHelper.getUserId(result, AuthType.TWITTER);     
        
        // do something with this vaultId and the twitterID
    }
}
```
{% endtab %}
{% endtabs %}
