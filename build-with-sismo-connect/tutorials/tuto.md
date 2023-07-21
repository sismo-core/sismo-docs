# Onchain Tutorial (1/2): Code Your Airdrop Contract With Privately-Aggregated Data

## Overview

This tutorial is designed as an **introduction** to **Sismo Connect Solidity** Library.&#x20;

It shows you how to create a Sybil-resistant ERC20 airdrop from privately-aggregated data. Take a look at the [**SafeDrop Case Study**](https://case-studies.sismo.io/db/safe-drop) to deeply understand the use case.

You will learn how to request ZK proofs about your user's data and verify them in your smart contracts.

## Prerequisites

This tutorial requires:

* [Node.js](https://nodejs.org/en/download/) >= 18.15.0 (Latest LTS version)
* [Yarn](https://classic.yarnpkg.com/en/docs/install)
* [Foundry](https://book.getfoundry.sh/) (see how to install it [here](https://book.getfoundry.sh/getting-started/installation))&#x20;
* MetaMask installed in your browser

{% hint style="info" %}
We use Foundry for our smart contract dependencies, but we also have a [**Hardhat library**](https://www.npmjs.com/package/@sismo-core/sismo-connect-solidity). A tutorial with Hardhat integration will be made in the coming weeks. Don't hesitate to ask questions on our [Builder Telegram](https://t.me/+Z-SwcvXZFRVhZTQ0) group if you have any.
{% endhint %}

## Installation

This tutorial is based on the following [**tutorial repository**](https://github.com/sismo-core/sismo-connect-onchain-tutorial). You can clone it with the following command:

<pre class="language-bash"><code class="lang-bash"><strong>git clone https://github.com/sismo-core/sismo-connect-onchain-tutorial
</strong>cd sismo-connect-onchain-tutorial
</code></pre>

#### Install contract dependencies

<pre class="language-bash"><code class="lang-bash"># updates foundry
foundryup
# install smart contract dependencies
<strong>forge install
</strong></code></pre>

#### Launch a local fork chain

```bash
# in another terminal
# starts a local fork of Mumbai
yarn chain
```

#### Launch the local application

You can now launch your local app with the commands:

```bash
# in another terminal
cd front
# install frontend dependencies
yarn
# launch local application and deploy smart contracts
yarn dev
```

This command starts the NextJs application that will call the contract in `src/Airdrop.sol` which has already been deployed on your local fork of Mumbai. You should now have the simple tutorial application running on [http://localhost:3000](http://localhost:3000).

You can now play with the local app that already integrates Sismo Connect. You should be able to claim your airdrop!

<figure><img src="../../.gitbook/assets/Capture d’écran 2023-06-16 à 23.38.53.png" alt=""><figcaption><p>Claim successfully your airdrop</p></figcaption></figure>

### Important note

{% hint style="warning" %}
The interaction with the fork network can become quite unstable if you stop the `yarn chain` command at some point or if you already used the sample app before.

You can end up with an infinitely pending transaction.

If so:

* keep the local anvil node running,&#x20;
* make sure to delete your activity tab for the fork network in MetaMask by going to "Settings > Advanced > Clear activity tab data" when connected to the fork network.&#x20;
* relaunch the anvil node and the application

See the [FAQ](../faq.md) for more information.
{% endhint %}

{% hint style="success" %}
This tutorial does not need any `anvil` or front-end restart; everything auto-reloads for you. You should only save the files you make changes to and play with your front end.
{% endhint %}

Congrats on getting your airdrop! Now, let's see how this simple Sismo Connect integration works behind the scenes and improve it along the way.

<figure><img src="../../.gitbook/assets/Sismo Connect onchain Flow.png" alt=""><figcaption></figcaption></figure>

### Sismo Connect Configuration

Go to the `front/src/app/page.tsx`

{% hint style="success" %}
To use Sismo Connect in your application, you will need to create an application  in the [Sismo Factory](https://factory.sismo.io/apps-explorer) and get its `appId`. You can see a quick tutorial on how to do it [here](create-a-sismo-connect-app.md).

For this tutorial, we will use an already existing app (appId: 0xf4977993e52606cfd67b7a1cde717069)
{% endhint %}

```typescript
// you are in: front/src/app/page.tsx

import {
  SismoConnectButton, // the Sismo Connect React button displayed below
  SismoConnectConfig,
  AuthType, 
  ClaimType
} from "@sismo-core/sismo-connect-react";

/* ***********************  Sismo Connect Config *************************** */

// you can create a new Sismo Connect app at https://factory.sismo.io
// The SismoConnectConfig is a configuration needed to connect to Sismo Connect and requests data from your users.

const sismoConnectConfig: SismoConnectConfig = {
  appId: "0xf4977993e52606cfd67b7a1cde717069",
  vault: {
    // We will impersonate those Data Sources, which will be useful later
    impersonate: [
      "leo21.sismo.eth",
      "0xA4C94A6091545e40fc9c3E0982AEc8942E282F38",
      "0x1b9424ed517f7700e7368e34a9743295a225d889",
      "0x82fbed074f62386ed43bb816f748e8817bf46ff7",
      "0xc281bd4db5bf94f02a8525dca954db3895685700",
      "twitter:leo21_eth",
      "github:leo21",
    ],
  },
};
```

You can spot the Sismo Connect configuration with the `appId` of the application created in [the Factory](https://factory.sismo.io). You can learn more about the Sismo Connect configuration [**here**](../technical-documentation/sismo-connect-configuration.md).

{% hint style="success" %}
Don't forget to remove impersonated accounts if you plan to deploy in production
{% endhint %}

### Create a Sismo Connect button&#x20;

The [**@sismo-core/sismo-connect-react**](../technical-documentation/packages/react.md) library simplifies the Sismo Connect integration since it offers a React button to easily request proofs of user data.&#x20;

<figure><img src="../../.gitbook/assets/sign in with Sismo Buttonx1.png" alt=""><figcaption><p>Sismo Connect React button</p></figcaption></figure>

Let's add the Sismo Connect Button and request  the`vaultId` of users.

<details>

<summary>vaultId: anonymous indentifier for a user. Will be used to avoid users to claim twice</summary>

Sismo users have a sovereign [Data Vault](../../how-sismo-works/core-components/what-is-the-data-vault.md) where they import Data Sources from which they will generate ZK proofs. Each Data Vault has a secret only known by its owner.

To identify users, you can request a `vaultId.`It is a sovereign and anonymous identifier natively provided to Sismo Connect Apps. vaultId = hash(userVaultSecret, AppId, 0)

The `vaultId` will be used to make sure a user that claimed the airdrop is registered and cannot claim it twice!

You can read more about it in [**Vault Identifiers.**](../technical-documentation/vault-and-proof-identifiers.md)&#x20;

</details>

```typescript

// you are still in: front/src/app/page.tsx
 
<SismoConnectButton
 // the Sismo Connect config created
 config={sismoConnectConfig}
 // the auth request we want to make
 // here we want to know the vaultId of our users for our application
 auths={[{ authType: AuthType.VAULT }]}
 // we ask the user to sign a message
 // it will be used onchain to prevent front running
 signature={{ message: signMessage(address) }}
 // onResponseBytes calls a 'setResponse' function 
 // with the responseBytes returned by the Sismo Vault
 onResponseBytes={(responseBytes: string) => {setResponseBytes(responseBytes)}}
 // Some text to display on the button
 text={"Claim with Sismo"}
/>
```

What are the different React button props?

* **`configuration.`**  Learn more about the **Sismo Connect Config** [**here**](../technical-documentation/sismo-connect-configuration.md).
* **`auths:`** Request proof of ownership of Data Sources here (Wallet, GitHub, Telegram, Twitter and vaultId).&#x20;
* **`signature:`** We can request users to sign a message; here, we request them to sign the recipient address for the airdrop, protecting them from front-running.

<details>

<summary>Front-running: why do we need a signature when verifying proofs onchain?</summary>

The signed message is not mandatory when you interact with your contracts, but it is very often needed. As far as your users are generating valid proofs, it could be quite easy for a third party to front-run them by taking their proof and making their own call to your smart contracts with it.

To overcome this issue, we offer a way to embed a specific message in a proof. It can be thought of as a signature since this proof could not be valid without checking successfully that the signed message is correct onchain.&#x20;

Here, for example, we request the user to embed the address where they want to receive the airdrop in the proof. If a third party takes the proof, the call will be reverted with a signature mismatch message, effectively protecting your users from being front-run.

</details>

* **`onResponseBytes:`** a function that will be called after the user has generated the ZK proof and sent the Sismo Connect Response. Here we are just storing the response.
* **`text:`** Content on the button

{% hint style="info" %}
For more: check the [**Cheatsheet**](../sismo-connect-cheatsheet.md), the [**@sismo-core/sismo-connect-react**](../technical-documentation/packages/react.md) library, and feel free to check the [**technical documentation**](../technical-documentation/packages/react.md).&#x20;
{% endhint %}

Now let's see how the contracts work!

## Verify proofs onchain

Let's verify the ZK proof in your contract. Go to `src/Airdrop.sol`

Below is the code that initiates the contract with the Sismo Connect Solidity Library, and like in the front end, it must be initiated with a configuration.

```solidity
// you are in: src/Airdrop.sol

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

import {ERC20} from "@openzeppelin/contracts/token/ERC20/ERC20.sol";
// Sismo Connect import
import "sismo-connect-solidity/SismoConnectLib.sol"; 

contract Airdrop is ERC20, SismoConnect { // <--- add a Sismo Connect inheritance
  // add your appId
  bytes16 private _appId = 0xf4977993e52606cfd67b7a1cde717069;
  // use impersonated mode for testing
  bool private _isImpersonationMode = true;
    
  constructor(
    string memory name,
    string memory symbol
  ) ERC20(name, symbol) 
  // Initiating the contract with the Sismo Connect Library, so it can verify 
 // Sismo Connect Responses (and their ZK Proofs)
    SismoConnect(buildConfig(_appId, _isImpersonationMode))
  {}
...
}
```

Take a look at the `claimWithSismo` function. It takes the Sismo Connect Response as an argument (which is received by the front after the user has generated the ZK proof) and verifies the response as valid against the authentication and the signature requested.&#x20;

If the response is valid, it checks that the airdrop has not been claimed before by the associated`vaultId` and finally mints 100 tokens to the address in the signature.&#x20;

<pre class="language-solidity"><code class="lang-solidity">// you are in src/Airdrop.sol

contract Airdrop is ERC20, SismoConnect {

    ....
    
<strong>  function claimWithSismo(bytes memory response) public {
</strong>    SismoConnectVerifiedResult memory result = verify({
      responseBytes: response,
      // we want the user to prove that he owns a Sismo Vault
      // we are recreating the auth request made in the front end to be sure that 
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

The `vaultId` enables protection against double spends in an anonymous way. Indeed, if a user generates a second proof from the same Vault, he would not be able to claim the airdrop again since each proof generated from a Vault has the same `vaultId` associated with it.&#x20;

{% hint style="success" %}
If you use Impersonation Mode, you will be able to claim the airdrop each time you try  since the `vaultId` is randomized in this mode.
{% endhint %}

The integration is basically done!&#x20;

Ok, so if we recap an onchain Sismo Connect integration, it all comes to:

* the creation of an app in the Sismo Factory to get an `appId`
* a Sismo Connect configuration in the front end and the smart contract with this `appId` and a possible "impersonation mode" enabled with chosen accounts for easy testing
* the configuration of the React button to choose which proofs to request from your users
* the creation of a smart contract to verify the user proofs onchain

Well, now that you have all these steps in mind, let's improve this airdrop contract by only allowing **Gitcoin Passport holders** to claim it.&#x20;

## Request proof of group membership

Our first aim is to make the ERC20 airdrop Sybil-resistant. To do this, we simply need to request a proof of Gitcoin Passport group membership from our users. We also want them to have a passport score above 15. You can request such a proof by taking the `groupId` of the "Gitcoin Passport Holders" group that can be found on the Sismo Factory at this link: [https://factory.sismo.io/groups-explorer?search=0x1cde61966decb8600dfd0749bd371f12](https://factory.sismo.io/groups-explorer?search=0x1cde61966decb8600dfd0749bd371f12) and create a [**claim request**](../technical-documentation/claims.md) from it.

{% hint style="success" %}
You can learn how to create a Data Group with the [following tutorial](../../data-groups/data-groups-and-creation/create-your-data-group.md).
{% endhint %}

The `groupId` of the Gitcoin Passport Holders group is `0x1cde61966decb8600dfd0749bd371f12`. Let's add our claim request in the React button. We indicate the groupId of the group and the minimum value required in this group.

```typescript
// you are in: front/src/app/page.tsx

export default function Home() {
 // add the groupId
 const GITCOIN_PASSPORT_HOLDERS_GROUP_ID = "0x1cde61966decb8600dfd0749bd371f12";
 
 ...
  
  return (
   ...
   
  <SismoConnectButton
   config={sismoConnectConfig}
   auths={[{ authType: AuthType.VAULT }]}
   // request a proof of Gitcoin Passport ownership from your users
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
}
```

Once you have created this claim request in the front end, you also need to add it to the smart contract. Always remember that the proofs sent to the smart contract need to be checked with respect to some requests.&#x20;

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

These simple code additions now allow our smart contract to only airdrop some tokens to holders of a Gitcoin Passport. While this is exciting, how can we quickly test it in our local front end if we are not Gitcoin Passport holders?&#x20;

The simplest solution is to impersonate an account holding a Gitcoin Passport. Remember that we are already impersonating the leo21.sismo.eth Ethereum account that holds a Gitcoin Passport, making us eligible.

{% hint style="success" %}
Don't forget to remove the `vault object` in the config when deploying in production.
{% endhint %}

When all of this is done, you can try again to go on your local application on [http://localhost:3000](http://localhost:3000) to see the new proofs requested.&#x20;

{% hint style="success" %}
**You don't need to do anything with anvil or your front-end application in your terminals. All is reloaded automatically for you. Just play with your front end by clicking on the "RESET" button in the top right! 😇**
{% endhint %}

As you can see below, you are now asked to share your `userId` like before, and you also should prove that you own a Gitcoin Passport. You also keep signing the address on which you want to receive the airdrop.



<figure><img src="../../.gitbook/assets/Capture d’écran 2023-07-19 à 10.12.43.png" alt=""><figcaption><p>Sismo Vault UI when redirected</p></figcaption></figure>

{% hint style="warning" %}
The interaction with the fork network can become quite unstable if you stop the `yarn chain` command at some point or if you already used the sample app before.

You can end up with an infinitely pending transaction.

If so:

* Keep the local anvil node running,&#x20;
* Make sure to delete your activity tab for the fork network in MetaMask by going to "Settings > Advanced > Clear activity tab data" when connected to the fork network.&#x20;
* Relaunch the anvil node and the application

See [FAQ](../faq.md) for more information.
{% endhint %}

Congrats again! You have successfully made your airdrop Sybil-resistant by gating it for holders of Gitcoin Passport.

Let's see how to ultimately combine data aggregation with privacy now by gating this Sybil-resistant airdrop to specific Sismo community members and allowing them to get more tokens depending on their reputation.

## Combine data aggregation and privacy

Data aggregation can be powerful in the context of an airdrop. For example, it can allow you to receive extra tokens if you prove some specific engagement in a community. In our example, we will offer more tokens to people that were early users of Sismo and to all the Sismo Factory users. Sismo Community members will also receive more tokens based on the value they have in the group.

To do this, we will simply do the same logic as before by creating additional claim requests for:

* Sismo Community members with the id `0xd630aa769278cacde879c5c0fe5d203c` (you can see the group description in the Sismo Factory [here](https://factory.sismo.io/groups-explorer?search=0xd630aa769278cacde879c5c0fe5d203c)). This membership is required.
* Early Sismo Community members with the id `0xe4c011331d91b79639df349a93157a1b` (you can see the group description in the Sismo Factory [here](https://factory.sismo.io/groups-explorer?search=0xe4c011331d91b79639df349a93157a1b)). This membership is optional and allows claimants to receive more tokens.
* Sismo Factory Users with the id `0x05629c9a54e30d8c8aea911a48cd9e30` (you can see the group description in the Sismo Factory [here](https://factory.sismo.io/groups-explorer?search=0x05629c9a54e30d8c8aea911a48cd9e30)). This membership is optional and allows users to receive more tokens.

You can add the claim requests in the front end. Notice that to claim the airdrop, you must have a Gitcoin Passport and be a member of the Sismo Community, but it is not required to be an early contributor or a Factory user.

```typescript
// you are in: front/src/app/page.tsx

export default function Home() {
 // add the groupIds
 const SISMO_COMMUNITY_MEMBERS_GROUP_ID = "0xd630aa769278cacde879c5c0fe5d203c";
 const EARLY_SISMO_COMMUNITY_MEMBERS_GROUP_ID = "0xe4c011331d91b79639df349a93157a1b";
 const SISMO_FACTORY_USERS_GROUP_ID = "0x05629c9a54e30d8c8aea911a48cd9e30";
 
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
    // the value of revealed can be selected by the user to gain more tokens
    { groupId: SISMO_COMMUNITY_MEMBERS_GROUP_ID, isSelectableByUser: true }, 
    // this proof of group membership optional
    { groupId: EARLY_SISMO_COMMUNITY_MEMBERS_GROUP_ID, isOptional: true }, 
    // this proof of group membership optional
    { groupId: SISMO_FACTORY_USERS_GROUP_ID, isOptional: true } 
   ]}
   signature={{ message: signMessage(address) }}
   onResponseBytes={(responseBytes: string) => setResponseBytes(responseBytes)}
   text={"Claim with Sismo"}
  />
  
   ...
 )
}
```

Add the claim requests in the smart contract:&#x20;

```solidity
// you are in src/Airdrop.sol

contract Airdrop is ERC20, SismoConnect {
    ...
    // add the groupIds
    bytes16 public constant SISMO_COMMUNITY_MEMBERS_GROUP_ID = 0xd630aa769278cacde879c5c0fe5d203c;
    bytes16 public constant EARLY_SISMO_COMMUNITY_MEMBERS_GROUP_ID = 0xe4c011331d91b79639df349a93157a1b;
    bytes16 public constant SISMO_FACTORY_USERS_GROUP_ID = 0x05629c9a54e30d8c8aea911a48cd9e30;
    ...
        
    function claimWithSismo(bytes memory response) public {
      AuthRequest[] memory auths = new AuthRequest[](1);
      auths[0] = buildAuth({authType: AuthType.VAULT});
  
      // add the claim request to check that the proof provided is valid
      // with respect to the Gitcoin Passport requirement
      // but also the Sismo Community requirement
      ClaimRequest[] memory claims = new ClaimRequest[](4);
      claims[0] = buildClaim({groupId: GITCOIN_PASSPORT_HOLDERS_GROUP_ID, value: 15, claimType: ClaimType.GTE});
      // the value can be selected by the user to gain more tokens
      claims[1] = buildClaim({groupId: SISMO_COMMUNITY_MEMBERS_GROUP_ID, isSelectableByUser: true, isOptional: false}); 
      // this proof of group membership optional
      claims[2] = buildClaim({groupId: EARLY_SISMO_COMMUNITY_MEMBERS_GROUP_ID, isSelectableByUser: false, isOptional: true}); 
      // this proof of group membership optional
      claims[3] = buildClaim({groupId: SISMO_FACTORY_USERS_GROUP_ID, isSelectableByUser: false, isOptional: true}); 
      
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
  
      // each vaultId can claim tokens relatively to their its aggregated reputation
      uint256 airdropAmount = _getRewardAmount(result);
  
      // we mark the user as claimed. We could also have stored more user airdrop information for a more complex airdrop system. But we keep it simple here.
      claimed[vaultId] = true;
  
      // we mint the tokens to the user
      _mint(msg.sender, airdropAmount);
    }
  ...
}
```

Add the new function to calculate the number of tokens to airdrop based on the aggregated reputation:

```solidity
// you are in src/Airdrop.sol

...

function _getRewardAmount(
  SismoConnectVerifiedResult memory result
) private pure returns (uint256) {
  uint256 airdropAmount = 0;
  uint256 REWARD_BASE_VALUE = 100 * 10 ** 18;

  // we iterate over the verified claims in the result
  for (uint i = 0; i < result.claims.length; i++) {
    bytes16 groupId = result.claims[i].groupId;
    uint256 value = result.claims[i].value;
    if (groupId == SISMO_COMMUNITY_MEMBERS_GROUP_ID) {
        // for SISMO_COMMUNITY_MEMBERS_GROUP_ID, the value is level in the community * 100 tokens
        airdropAmount += value * REWARD_BASE_VALUE;
    } else {
        // for all other groups, the value is 100 tokens
        airdropAmount += REWARD_BASE_VALUE;
    }
  }
  return airdropAmount;
}
```

You can try again to claim the airdrop from your application. You will see the auth request with the four claim requests and the sign message. If you share all the information by default, you should end up with 400 tokens!



<figure><img src="../../.gitbook/assets/Capture d’écran 2023-07-19 à 10.15.44.png" alt=""><figcaption><p>Sismo Vault UI when redirected</p></figcaption></figure>

And it is our final congrats! 🎉

If we recap, you managed to go from a simple request of **vaultId** to a complex multi-request of **vaultId** + **group membership** allowing you to create a Sybil-resistant airdrop from privately-aggregated data. All these requests respect user privacy since no Ethereum address is ever shared during the flow, thanks to data aggregation in Sismo's Data Vault.

To see how to deploy your contracts, you can go to the [**associated tutorial**](deploy-your-contracts.md).

## Next steps

If you have any questions about integrating Sismo Connect, don’t hesitate to reach out. The team will be happy to answer any questions you may have. Any feedback is also welcomed! If you want to fork a repository with complex requests already set up, you can check our [**onchain boilerplate**](broken-reference).

Get involved in the Sismo community! 🎭

* Look out for [**hackathons**](https://www.notion.so/sismo/Sismo-x-ETHPorto-2023-cbda827ea5f2469aa5fdbb4955fc18d6?pvs=4) that we are participating in
* Join our [**Discord**](https://discord.gg/sismo) or our [**Dev** **Telegram**](https://t.me/+Z-SwcvXZFRVhZTQ0)
* See the [**Sismo-hub contributing page**](https://github.com/sismo-core/sismo-hub/issues)
