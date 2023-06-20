# Build a Sybil-resistant Gated Airdrop

## Overview

This tutorial is designed as an **introduction** to **Sismo Connect Solidity** Library. It aims at showcasing the integration steps of Sismo Connect to easily create a Sybil-resistant gated airdrop to Sismo Community members while explaining some useful concepts of the Sismo Connect communication protocol.&#x20;

You will learn how to request proofs about your users data, verify them in your contracts and how privacy and data aggregation can be leveraged for your app thanks to Sismo Connect.

If you wish to understand what is Sismo Connect from a high level view, we advise you to read about it in the [Discover Sismo Connect section](../../../welcome-to-sismo/what-is-sismo-connect.md).

## Prerequisites

This tutorial requires:

* [Node.js](https://nodejs.org/en/download/) >= 18.15.0 (Latest LTS version)
* [Yarn](https://classic.yarnpkg.com/en/docs/install)
* [Foundry](https://book.getfoundry.sh/) (see how to install it [here](https://book.getfoundry.sh/getting-started/installation))&#x20;
* Metamask installed in your browser

{% hint style="info" %}
We use Foundry for our smart contract dependencies but we also have a [**Hardhat library**](https://www.npmjs.com/package/@sismo-core/sismo-connect-solidity). A tutorial with Hardhat integration will be made in the coming weeks. Don't hesitate to ask questions on our [Builder Telegram](https://t.me/+Z-SwcvXZFRVhZTQ0) group if you have any.
{% endhint %}

## Installation

This tutorial is based on the following [**tutorial repository**](https://github.com/sismo-core/sismo-connect-onchain-tutorial). You can clone it with the following command:

<pre class="language-bash"><code class="lang-bash"><strong>git clone https://github.com/sismo-core/sismo-connect-onchain-tutorial
</strong>cd sismo-connect-onchain-tutorial
yarn
</code></pre>

#### Install contract dependencies

```bash
# updates foundry
foundryup
# install smart contract dependencies
forge install
```

#### Launch a local fork chain

```bash
# in another terminal
# starts a local fork of Mumbai
yarn anvil
```

#### Launch the local application

You can now launch your local dapp with the commands:

```bash
# in another terminal
cd front
# install frontend dependencies
yarn
# launch local application
yarn dev
```

This command starts the NextJs application that calls the contract in `src/Airdrop.sol` that has been deployed on your local fork of Mumbai. You should now have the simple tutorial application running on [http://localhost:3000](http://localhost:3000).

<figure><img src="../../../.gitbook/assets/Capture d’écran 2023-06-16 à 23.41.13.png" alt=""><figcaption><p>Your local frontend</p></figcaption></figure>

You can now play with the local app that already integrates Sismo Connect by connecting your wallet and signing in with Sismo.

Once you are redirected from your Sismo Vault, you can click on the "Claim" button, you are then shown that you have successfully claim the airdrop.&#x20;

<figure><img src="../../../.gitbook/assets/Capture d’écran 2023-06-16 à 23.38.53.png" alt=""><figcaption><p>Claim successfully your airdrop</p></figcaption></figure>

### Important note

{% hint style="warning" %}
The interaction with the fork network can become quite unstable if you stop the `yarn anvil` command at some point or if you already use the sample app before.

You can end up with a infinite pending transaction.

If so:

* keep the local anvil node running,&#x20;
* make sure to delete your activity tab for the fork network in Metamask by going to "Settings > Advanced > Clear activity tab data" when connected to the fork network.&#x20;
* relaunch the anvil node and the application

See [FAQ](../../faq.md) for more informations.
{% endhint %}

{% hint style="success" %}
This tutorial does not need any `anvil` restart or frontend restart, everything auto reloads for you. You should only save the files you make changes to and play with your frontend.
{% endhint %}

Congrats on getting your airdrop! Now, let's see how this simple Sismo Connect integration works behind the scene and let's improve it along the way. Let's start with the authentication you just experimented.

## Authenticate your users

Among other things, Sismo Connect allows a simple user authentication for your application. It works by requesting an authentication (shorten to **auth**) of type **VAULT** to your users. What does it mean?&#x20;

Each Sismo user has a Data Vault, where all his data is stored securely. As a developer, you can request to your users some proofs about the data in their Vault thanks to a Sismo Connect Request. In the case of an auth of type VAULT, you request a proof of ownership of a Sismo Vault from your users. When they are done generating the proof in their vault, they are redirected to your application with a Sismo Connect Response that holds the proof but also a `vaultId`.&#x20;

The **`vaultId`** is the **unique identifier of a user vault for a specific application**, it is computed as the **hash** of the **userVaultSecret** and the **appId**. If you want to learn more about the `vaultId`, you can read more about it in [Vault & Proof Identifiers](../../technical-documentation/vault-and-proof-identifiers.md).

You can also see a general scheme below showing you the high-level workflow:

<figure><img src="../../../.gitbook/assets/Sismo Connect onchain Flow.png" alt=""><figcaption><p>Sismo Connect onchain Flow</p></figcaption></figure>

### Create a Sismo Connect configuration

To see how we created the auth request, you can go to the `front/src/app/page.tsx` file. The first thing we do is defining a **Sismo Connect configuration**, this config will let the Sismo Vault app knows about the `appId` from which the requests are made. In this tutorial, we use `0xf4977993e52606cfd67b7a1cde717069` for the `appId`.

{% hint style="success" %}
In order to use Sismo Connect in your application, you will first need to create an application  in the [Sismo Factory](https://factory.sismo.io/apps-explorer) and get its `appId`. You can see a quick tutorial on how to do it [here](../create-a-sismo-connect-app.md).
{% endhint %}

```typescript
// you are in: front/src/app/page.tsx

import {
  SismoConnectButton, // the Sismo Connect React button displayed below
  SismoConnectConfig, // the Sismo Connect config with your appId
  AuthType, // the authType enum, we will choose 'VAULT' in this tutorial
  ClaimType // the claimType enum, we will choose 'GTE' in this tutorial, to check that the user has a value greater than a given threshold
} from "@sismo-core/sismo-connect-react";

/* ***********************  Sismo Connect Config *************************** */

// you can create a new Sismo Connect app at https://factory.sismo.io
// The SismoConnectConfig is a configuration needed to connect to Sismo Connect and requests data from your users.

const sismoConnectConfig: SismoConnectConfig = {
  appId: "0xf4977993e52606cfd67b7a1cde717069",
  vault: {
    // For development purposes
    // insert any account that you want to impersonate  here
    // Never use this in production
    impersonate: [
      "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", // vitalik.eth
    ],
  },
};
```

You can spot in the snippets all the imports needed for the tutorial to work smoothly and the Sismo Connect configuration that you create with the `appId` from the Factory.

{% hint style="success" %}
The `vault object` in the configuration should be omitted if you are in production mode, it enables you to specify which accounts you want to impersonate while developing your Sismo Connect app in order to generate impersonated proofs. In this tutorial, we are impersonating vitalik.eth ethereum address to generate proofs.
{% endhint %}

### Create a Sismo Connect button&#x20;

The [**@sismo-core/sismo-connect-react**](../../technical-documentation/packages/react.md) library simplifies the Sismo Connect integration since it offers a React button to easily request proofs of your users data.&#x20;

<figure><img src="../../../.gitbook/assets/sign in with Sismo Buttonx1.png" alt=""><figcaption><p>Sismo Connect React button</p></figcaption></figure>

The configuration is now helpful to setup properly our Sismo Connect button. You will see below the full button props that are explained in detail after the code snippet.

```typescript

// you are still in: front/src/app/page.tsx
 
<SismoConnectButton
 // the Sismo Connect config created
 config={sismoConnectConfig}
 // the auth request we want to make
 // here we want the proof of a Sismo Vault ownership from our users
 auths={[{ authType: AuthType.VAULT }]}
 // we ask the user to sign a message
 // it will be used onchain to prevent front running
 signature={{ message: signMessage(address) }}
 // onResponseBytes calls a 'setResponse' function 
 // with the responseBytes returned by the Sismo Vault
 onResponseBytes={(responseBytes: string) => setResponseBytes(responseBytes)}
 // Some text to display on the button
 text={"Claim with Sismo"}
/>
```

What are the different react button props?

* the **`config`** prop will let the Sismo Vault app knows about the `appId`  from which the requests are made.&#x20;
* the **`auths`** prop defines the different auth requests we want our users to prove ownership of (in this case, that they own a Vault at Sismo).&#x20;
* the **`signature`** prop dictates which message should be signed by the user when generating the proof in there Sismo Vault (here we want them to sign the address on which they want to receive the airdrop).&#x20;

{% hint style="success" %}
By making the user sign this message and by checking the signature in the contract, we ensure that the proof cannot be reused from someone else. It is basically a protection from being front run.
{% endhint %}

<details>

<summary>Why do we need a signature when verifying proofs on-chain?</summary>

The signed message is not mandatory when you interact with your contracts but it is very often needed. As far as your users are generating valid proofs, it could be quite easy for a third party to front run them by just taking their proof and make their own call to your smart contracts with it.

To overcome this issue, we offer a way to embed a specific message in a proof. This way it can be thought as a signature since this proof could not be valid without checking successfully that the signed message is correct on-chain.&#x20;

Here for example, we request the user to embed the address where they want to receive the airdrop in the proof. If a third party takes the proof, the call will be reverted with a signature mismatch message, effectively protecting your users from being front run.

</details>

* the **`onResponseBytes`** prop specifies which logic to trigger when we receive the Sismo Connect response (holding the proofs) from the Sismo Vault app (here we just want to save the response in a React state to use it after when calling the contract).
* the **`text`** prop defines which text to display on the button (default being "Sign in with Sismo")

You should have a pretty good understanding of the button at this point but if you want to see the complete documentation around the [**@sismo-core/sismo-connect-react**](../../technical-documentation/packages/react.md) library, feel free to check the [**technical documentation**](../../technical-documentation/packages/react.md).&#x20;

Now let's see how the contracts are working!

## Verify proofs onchain

Now that you know how to request proofs from your users data, you still need to know how to verify them. You can go to `src/Airdrop.sol` to see the contract code.

```solidity
// you are in: src/Airdrop.sol

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

import {ERC20} from "@openzeppelin/contracts/token/ERC20/ERC20.sol";
// Sismo Connect import
import "sismo-connect-solidity/SismoLib.sol"; 
```

You will start by inheriting from Sismo Connect by importing the library, add the inheritance and specify your `appId` in the contract to be able to pass it in the constructor. If you wish to use the impersonation mode while testing, you also need to specify it.

{% hint style="success" %}
The `buildConfig` in the constructor is an helper in the Sismo Connect Solidity library that allows you to easily configure your contract by at least specifying an `appId`.
{% endhint %}

```solidity
// you are in: src/Airdrop.sol

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

import {ERC20} from "@openzeppelin/contracts/token/ERC20/ERC20.sol";
// Sismo Connect import
import "sismo-connect-solidity/SismoLib.sol"; 

contract Airdrop is ERC20, SismoConnect { // <--- add a Sismo Connect inheritance
  // add your appId as a constant
  bytes16 public constant APP_ID = 0xf4977993e52606cfd67b7a1cde717069;
  // use impersonated mode for testing
  bool public constant IS_IMPERSONATION_MODE = true;
    
  constructor(
      string memory name,
      string memory symbol
  ) ERC20(name, symbol) 
  // add a Sismo Connect constructor with the buildConfig helper inside it
    SismoConnect(buildConfig(APP_ID, IS_IMPERSONATION_MODE))
  {}
...
}
```

You can then take a look at the `claimWithSismo` function that we call from the frontend. It takes a Sismo Connect Response as `bytes`  as argument, verifies the proof it holds with respect to the auth request and the signature expected. If the proof is valid, it gets the `vaultId` from the Sismo Connect Verified Result, verifies that the airdrop has not been claimed before and finally mints 100 token on the address in the signature. The `vaultId` being the anonymous identifier of a user's vault for a specific app, defined as hash(`userVaultSecret`, `appId`).

What is interesting here is that the `vaultId` is a protection from double spendings. Indeed, if a user generates a second proof from the same vault he would not be able to claim the airdrop again since each proof generated from a Vault has the same `vaultId` associated to it.&#x20;

<pre class="language-solidity"><code class="lang-solidity">// you are in src/Airdrop.sol

contract Airdrop is ERC20, SismoConnect {

    ....
    
<strong>  function claimWithSismo(bytes memory response) public {
</strong>    SismoConnectVerifiedResult memory result = verify({
      responseBytes: response,
      // we want the user to prove that he owns a Sismo Vault
      // we are recreating the auth request made in the frontend to be sure that 
      // the proofs provided in the response are valid with respect to this auth request
      auth: buildAuth({authType: AuthType.VAULT}),
      // we also want to check if the signed message provided in the response is the signature of the user's address
      signature:  buildSignature({message: abi.encode(msg.sender)})
    });

    // if the proofs and signed message are valid, we take the userId from the verified result
    // in this case the userId is the vaultId (since we used AuthType.VAULT in the auth request), 
    // it is the anonymous identifier of a user's vault for a specific app 
    // --> vaultId = hash(userVaultSecret, appId)
    uint256 vaultId = result.getUserId(AuthType.VAULT);

    // we check if the user has already claimed the airdrop
    if (claimed[vaultId]) {
      revert AlreadyClaimed();
    }
    // each vaultId can claim 100 tokens
    uint256 airdropAmount = 100 * 10 ** 18;

    // we mark the user as claimed. We could also have stored more user airdrop information for a more complex airdrop system. But we keep it simple here.
    claimed[vaultId] = true;

    // we mint the tokens to the user
    _mint(msg.sender, airdropAmount);
  }
    ....
}
</code></pre>

The integration is basically done!&#x20;

Ok, so if we recap an onchain Sismo Connect integration, it all comes to:

* the creation of an app in the Sismo Factory, to get an `appId`
* a Sismo Connect configuration in the frontend and the smart contract with this `appId` and a possible "impersonation mode" enabled with chosen accounts for easy testing
* the configuration of the React button to choose which proofs to requests from your users
* the creation of a smart contract to verify the user proofs onchain

Well, now that you have all this steps in mind, let's improve this airdrop contract by only allowing **Gitcoin Passport holders** to claim it.&#x20;

## Request proof of group membership

Our first aim is to make the ERC20 Aidrop Sybil-resistant, to do this we simply need to request a proof of Gitcoin Passport group membership from our users. We also want them to have a passport score above 15. You can request such a proof by taking the `groupId` of the "Gitcoin Passport Holders" group that can be found on the Sismo Factory at this link: [https://factory.sismo.io/groups-explorer?search=gitcoin-passport-holders](https://factory.sismo.io/groups-explorer?search=gitcoin-passport-holders) and create a **claim request** from it.

The `groupId` of the Gitcoin Passport Holders group is `0x1cde61966decb8600dfd0749bd371f12`. Let's add our claim request in the React button. We indicate the groupId of the group and the minimum value required in this group.

```typescript
// you are in: front/src/app/page.tsx

export default function ClaimAirdrop() {
// add the groupId
const GITCOIN_PASSPORT_HOLDERS_GROUP_ID = "0x1cde61966decb8600dfd0749bd371f12";

...
 
 return (
  ...
  
 <SismoConnectButton
  config={sismoConnectConfig}
  auths={[{ authType: AuthType.VAULT }]}
  // request a proof of group membership from your users
  // They should hold a Gitcoin Passport
  // pass the groupId and the minimum value required in the group
  claims={[{ 
   groupId: GITCOIN_PASSPORT_HOLDERS_GROUP_ID, 
   value: 15, 
   claimType: ClaimType.GTE
  }]} 
  signature={{ message: signMessage(address) }}
  onResponseBytes={(responseBytes: string) => setResponseBytes(responseBytes)}
  text={"Claim with Sismo"}
 />
 
  ...
)
```

Once you have created this claim request in the frontend, you also need to add it in the smart contract. Always remember that the proofs sent to the smart contract need to be checked with respect to some requests.&#x20;

<pre class="language-solidity"><code class="lang-solidity">// you are in src/Airdrop.sol

contract Airdrop is ERC20, SismoConnect {
    ...
    // add your groupId as a constant
    bytes16 public constant GITCOIN_PASSPORT_HOLDERS_GROUP_ID = 0x1cde61966decb8600dfd0749bd371f12;
        
    ...
        
<strong>    function claimWithSismo(bytes memory response) public {
</strong>      SismoConnectVerifiedResult memory result = verify({
        responseBytes: response,
        // we want the user to prove that he owns a Sismo Vault
        // we are recreating the auth request made in the frontend to be sure that 
        // the proofs provided in the response are valid with respect to this auth request
        auth: buildAuth({authType: AuthType.VAULT}),
        claim: buildClaim({
                  groupId: GITCOIN_PASSPORT_HOLDERS_GROUP_ID, 
                  value: 15, 
                  claimType: ClaimType.GTE
              }),
        // we also want to check if the signed message provided in the response is the signature of the user's address
        signature:  buildSignature({message: abi.encode(msg.sender)})
      });
  
      // if the proofs and signed message are valid, we take the userId from the verified result
      // in this case the userId is the vaultId (since we used AuthType.VAULT in the auth request), 
      // it is the anonymous identifier of a user's vault for a specific app 
      // --> vaultId = hash(userVaultSecret, appId)
      uint256 vaultId = result.getUserId(AuthType.VAULT);
  
      // we check if the user has already claimed the airdrop
      if (claimed[vaultId]) {
        revert AlreadyClaimed();
      }
      // each vaultId can claim 100 tokens
      uint256 airdropAmount = 100 * 10 ** 18;
  
      // we mark the user as claimed. We could also have stored more user airdrop information for a more complex airdrop system. But we keep it simple here.
      claimed[vaultId] = true;
  
      // we mint the tokens to the user
      _mint(msg.sender, airdropAmount);
    }
    ...
}
</code></pre>

These simple code additions now allow our smart contract to only airdrop some tokens to holders of a Gitcoin Passport. While this is exciting, how can we quickly test it in our local frontend if we are not a Gitcoin Passport holder?&#x20;

The simplest solution is to impersonate an account holding a Gitcoin Passport. You can quickly find one by going [**here**](https://factory.sismo.io/groups-explorer?search=gitcoin-passport-holders) and clicking on **"Number of eligible users",** we will use this address in our case: `0xce2ef28c65e5db090d75630c98a807da003fb36f`

{% hint style="success" %}
Don't forget to remove the `vault object` in the config when deploying in production.
{% endhint %}

```typescript
// you are in: front/src/app/page.tsx

const sismoConnectConfig: SismoConnectConfig = {
  appId: "0xf4977993e52606cfd67b7a1cde717069",
  vault: {
    // For development purposes
    // insert any account that you want to impersonate  here
    // Never use this in production
    impersonate: [
      "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", // vitalik.eth
      "0xce2ef28c65e5db090d75630c98a807da003fb36f", // a Gitcoin Passport Holder
    ],
  },
};
```

When all of this is done, you can try again to go on your local application on [http://localhost:3000](http://localhost:3000) to see the new proofs requested.&#x20;

{% hint style="success" %}
**You don't need to do anything with anvil or your frontend application in your terminals, all is reloaded automatically for you. Just play with your front by clicking on the "RESET" button in the top right! 😇**
{% endhint %}

As you can see below, you are now asked to share your `userId` like before and you also should prove that you own a Gitcoin Passport. You also keep signing the address on which you want to receive the airdrop. Since Vitalik.eth has no Gitcoin Passport on his address, we needed to impersonate a Gitcoin Passport holder to generate a proof. Quite easy right?

<figure><img src="../../../.gitbook/assets/Capture d’écran 2023-06-16 à 23.53.45.png" alt=""><figcaption><p>Sismo Vault UI when redirected</p></figcaption></figure>

{% hint style="warning" %}
The interaction with the fork network can become quite unstable if you stop the `yarn anvil` command at some point or if you already use the sample app before.

You can end up with a infinite pending transaction.

If so:

* keep the local anvil node running,&#x20;
* make sure to delete your activity tab for the fork network in Metamask by going to "Settings > Advanced > Clear activity tab data" when connected to the fork network.&#x20;
* relaunch the anvil node and the application

See [FAQ](../../faq.md) for more informations.
{% endhint %}

Congrats again! You have successfully made your airdrop sybil-resistant by gating it for holders of Gitcoin Passport.

Let's see how to ultimately combine data aggregation with privacy now by gating this sybil-resistant airdrop to Sismo Community members.

## Combine data aggregation and privacy

To do this, we will simply do the same logic as before by creating an additional claim request for members of Sismo Community members with the id `0xd630aa769278cacde879c5c0fe5d203c` (you can see the group description in the Sismo Factory [here](https://factory.sismo.io/groups-explorer?search=0xd630aa769278cacde879c5c0fe5d203c)).

You can add the claim request in the frontend:

```typescript
// you are in: front/src/pages/claim-airdrop.tsx

export default function ClaimAirdrop() {
// add the groupId
const SISMO_COMMUNITY_MEMBERS_GROUP_ID = "0xd630aa769278cacde879c5c0fe5d203c";

...
 
 return (
  ...
  
 <SismoConnectButton
  config={sismoConnectConfig}
  auths={[{ authType: AuthType.VAULT }]}
  // request an additional proof of group membership from your users
  // They should be part of the Sismo Community
  // but also hold a Gitcoin Passport with a minimum value of 15 as before
  claims={[
   { groupId: GITCOIN_PASSPORT_HOLDERS_GROUP_ID, value: 15, claimType: ClaimType.GTE },
   { groupId: SISMO_COMMUNITY_MEMBERS_GROUP_ID } // <-- pass the groupId
  ]}
  signature={{ message: signMessage(address) }}
  onResponseBytes={(responseBytes: string) => setResponseBytes(responseBytes)}
  text={"Claim with Sismo"}
 />
 
  ...
)
```

Add the claim request in the smart contract:&#x20;

```solidity
// you are in src/Airdrop.sol

contract Airdrop is ERC20, SismoConnect {
    ...
    // add your groupId as a constant
    bytes16 public constant SISMO_COMMUNITY_MEMBERS_GROUP_ID = 0xd630aa769278cacde879c5c0fe5d203c;
        
    ...
        
    function claimWithSismo(bytes memory response) public {
      AuthRequest[] memory auths = new AuthRequest[](1);
      auths[0] = buildAuth({authType: AuthType.VAULT});
  
      // add the claim request to check that the proof provided is valid
      // with respect to the Gitcoin Passport requirement
      // but also the Sismo Community requirement
      ClaimRequest[] memory claims = new ClaimRequest[](2);
      claims[0] = buildClaim({groupId: GITCOIN_PASSPORT_HOLDERS_GROUP_ID, value: 15, claimType: ClaimType.GTE});
      claims[1] = buildClaim({groupId: SISMO_COMMUNITY_MEMBERS_GROUP_ID}); // <-- pass the groupId
        
      SismoConnectVerifiedResult memory result = verify({
        responseBytes: response,
        auths: auths,
        claims: claims,
        signature:  buildSignature({message: abi.encode(msg.sender)})
      });
  
      // if the proofs and signed message are valid, we take the userId from the verified result
      // in this case the userId is the vaultId (since we used AuthType.VAULT in the auth request), 
      // it is the anonymous identifier of a user's vault for a specific app 
      // --> vaultId = hash(userVaultSecret, appId)
      uint256 vaultId = result.getUserId(AuthType.VAULT);
  
      // we check if the user has already claimed the airdrop
      if (claimed[vaultId]) {
        revert AlreadyClaimed();
      }
  
      // each vaultId can claim 100 tokens
      uint256 airdropAmount = 100 * 10 ** 18;
  
      // we mark the user as claimed. We could also have stored more user airdrop information for a more complex airdrop system. But we keep it simple here.
      claimed[vaultId] = true;
  
      // we mint the tokens to the user
      _mint(msg.sender, airdropAmount);
    }
  ...
}
```

Once you have done that, you can impersonate an account that is a [**Sismo Community member**](https://factory.sismo.io/groups-explorer?search=sismo-community). Remember to not let such configuration in a production environment!

```typescript
// you are in: front/src/app/page.tsx

const sismoConnectConfig: SismoConnectConfig = {
  appId: "0xf4977993e52606cfd67b7a1cde717069",
  vault: {
    // For development purposes
    // insert any account that you want to impersonate  here
    // Never use this in production
    impersonate: [
      "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", // vitalik.eth
      "0xce2ef28c65e5db090d75630c98a807da003fb36f", // a Gitcoin Passport Holder
      "0x855193BCbdbD346B423FF830b507CBf90ecCc90B", // the address of a Sismo team member
    ],
  },
};
```

You can try again to claim the airdrop from your application, you will see the auth request with the two claim requests and the sign message.&#x20;

<figure><img src="../../../.gitbook/assets/Capture d’écran 2023-06-17 à 00.11.07.png" alt=""><figcaption><p>Sismo Vault UI when redirected</p></figcaption></figure>

And it is our final congrats! 🎉

If we recap, you basically managed to go from a simple request of Vault ownership to a complex multi request of Vault ownership + group memberships allowing you to create a sybil-resistant gated airdrop to Sismo Community members. All these requests respect the user privacy since no ethereum address is ever shared during the flow and all of this is made possible thanks to the data aggregation that offers the Sismo Vault.&#x20;

To see how to deploy your contracts, you can go to the [**associated tutorial**](deploy-your-contracts.md).

## Next steps

If you have any questions about integrating sismoConnect, don’t hesitate to reach out. The team will be happy to answer any questions you may have. Any feedback is also welcomed! If you want to fork a repository with complex requests already setup, you can check our [**onchain boilerplate**](../../run-example-apps/onchain-sample-project.md).

Get involved in the Sismo community! 🎭

* Look out for [**hackathons**](https://www.notion.so/sismo/Sismo-x-ETHPorto-2023-cbda827ea5f2469aa5fdbb4955fc18d6?pvs=4) that we are participating in
* Join our [**Discord**](https://discord.gg/sismo) or our [**Dev** **Telegram**](https://t.me/+Z-SwcvXZFRVhZTQ0)
* See the [**Sismo-hub contributing page**](https://github.com/sismo-core/sismo-hub/issues)
