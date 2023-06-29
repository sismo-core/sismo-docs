---
description: Verify proofs from your users
---

# Sismo Connect Server: Verify Offchain

The Sismo Connect Server is a backend package built on top of the [Hydra-S2 Verifier](../../../how-sismo-works/technical-concepts/proving-schemes/hydra-s2.md) to easily verify proofs from your users off-chain.&#x20;

<figure><img src="../../../.gitbook/assets/Sismo Connect offchain Flow.png" alt=""><figcaption><p>Sismo Connect offchain Flow</p></figcaption></figure>

## Installation

Install [Sismo Connect](../../../welcome-to-sismo/what-is-sismo-connect.md) server package in your backend with `npm` or `yarn`:

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

The first step for integrating Sismo Connect in your backend is to create a `sismoConnectServerConfig`. This config will require an `appId` and can be customized with optional fields. You can go to the [Sismo Factory](https://factory.sismo.io/apps-explorer) to register an appId ([here is a tutorial](../../tutorials/create-a-sismo-connect-app.md)).

This `config` is then used to create a `SismoConnect` instance that will be used to verify proofs.

<pre class="language-typescript"><code class="lang-typescript"><strong>import { SismoConnect, SismoConnectConfig } from "@sismo-core/sismo-connect-server";
</strong>
const config: SismoConnectConfig = {
  // you will need to register an appId in the Factory
  appId: "0x8f347ca31790557391cec39b06f02dc2",
}

// create a new Sismo Connect instance with the server configuration
const sismoConnect = SismoConnect({ config });
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

##
