# Gate your contracts with Sismo Connect (Advanced)

### What’s inside?

This tutorial will walk you through how we built zkDrop, an application allowing whitelisted wallets and GitHub accounts to benefit from an airdrop on a public address while keeping their eligible account private thanks to an on-chain [**Sismo Connect**](../../readme/sismo-connect.md) integration.

To get a feel of what you will build in this tutorial, you can try out a demo [**here**](https://demo.zkdrop.io/mergooor-pass).

{% hint style="info" %}
You can access the open-source repository of the demo [here](https://github.com/sismo-core/zkdrop).

The repository use the [`@sismo-core/sismo-connect-react`](../../technical-documentation/sismo-connect/react.md) package to request the proof and the [Sismo Connect Solidity library](../../technical-documentation/sismo-connect/solidity-library.md) to verify it.&#x20;
{% endhint %}

### Tutorial use case

We will use the example of [The Merge](https://ethereum.org/en/upgrades/merge/) contributors and [the initiative of Tim Beiko](https://twitter.com/zkdrop\_io/status/1615716498000977921?s=20) to airdrop to contributors to The Merge a ZK Soulbound token (here an ERC721) to allow them to join events hosted by [ETHGlobal](https://ethglobal.com/), [EthCC](https://www.ethcc.io/), [Edcon](https://www.edcon.io/) and [Devcon](https://devcon.org/).

For anonymous contributors to The Merge, we will build zkDrop that enables them to claim the airdrop on a public address in a privacy-preserving manner. These anonymous contributors will then be able to join events with this ERC721.

{% hint style="info" %}
We definitely recommend our entry-point tutorial for a quick off-chain Sismo Connect integration [here](authenticate-your-users-with-sismo-connect.md). Feel free to take a look or even try it first before following this more advanced one.
{% endhint %}

### Requirements

To implement Sismo Connect, you'll need both a frontend and some smart contracts using [Foundry](https://book.getfoundry.sh/). The frontend will request the proof, and the smart contracts will verify it on-chain thanks to the Sismo Connect Solidity library.

You can install Foundry by following this guide [https://book.getfoundry.sh/getting-started/installation](https://book.getfoundry.sh/getting-started/installation)&#x20;

For the frontend part of the application you will need to install:

* [Node.js](https://nodejs.org/en/download/) (v18.15.0, latest LTS version)
* [Yarn](https://classic.yarnpkg.com/en/docs/install/#mac-stable)
* [Foundry](https://book.getfoundry.sh/getting-started/installation)

### Setup your repository

To setup your repository with foundry we recommend you to use our zkDrop repository as a foundry template [https://github.com/sismo-core/zkdrop](https://github.com/sismo-core/zkdrop)

```bash
forge init --template https://github.com/sismo-core/zkdrop zkdrop-tutorial

# Go to zkdrop-tutorial folder
cd zkdrop-tutorial
```

For the frontend part, we are using the Next.js stack, which is a full-stack React framework. Next.js also offers a deployment service called Vercel, which makes it easy to deploy your app in just two clicks.

Your repository has been initiated on the zkdrop-tutorial folder.

Install dependencies:

```bash
# Install forge dependencies
forge install

# Install front dependencies
cd front
yarn
```

That's it! Your contracts will be located in the `src` folder and your frontend will be located in the `front` folder.&#x20;

{% hint style="info" %}
You can also inspire you from our boilerplate repository: [https://github.com/sismo-core/sismo-connect-boilerplate](https://github.com/sismo-core/sismo-connect-boilerplate)
{% endhint %}

### Register your Sismo Connect App in the Factory

Before you begin integrating [**Sismo Connect**](../../readme/sismo-connect.md), you must create first a Sismo Connect app in the [**Sismo Factory**](https://factory.sismo.io/apps-explorer). This step is mandatory to obtain an application Id (`appId`), which is required during the Sismo Connect development process.

<details>

<summary>Why is an <code>appId</code> mandatory for Sismo Connect?</summary>

The `appId` will be used to compute an VaultId, which is the the unique identifier for a user on your app. The VaultId is simply the hash of a user's Vault secret and the appId.

$$vaultId = hash(vaultSecret, appId)$$

If we remove the appId from this simple calculation, we would have had the same VaultId for the same vaultSecret, effectively leaking information about a user that uses Sismo Connect on two different apps. The VaultId would be the same across different apps, and the user could be tracked if the VaultIds became public.

By introducing an appId, the vaultId is now different between apps, and the same user will have two different VaultIds on two different apps, effectively preserving the user's privacy.

You can learn more about this notion in this [article](../../technical-concepts/vault-and-proof-identifiers.md).

</details>

<figure><img src="../../.gitbook/assets/Capture d’écran 2023-03-14 à 19.47.52 (1) (1) (1).png" alt=""><figcaption><p>Register your Sismo Connect App in the <a href="https://factory.sismo.io/apps-explorer">Sismo Factory</a></p></figcaption></figure>

You can register a Sismo Connect app here: [https://factory.sismo.io/apps-explorer](https://factory.sismo.io/apps-explorer).\
\
To create a Sismo Connect app, you need to log in with Sign-In With Ethereum and click on “create a new Sismo Connect app”. You will need to register an App Name, enter a description, and upload a logo alongside registering authorized domains. Pay attention to authorized domains, as these are the urls where the appId that will be created can be used for [Sismo Connect](../../technical-documentation/sismo-connect/).

{% hint style="info" %}
Feel free to add `*.com` to authorized domains when following along this tutorial. This will allow to whitelist `localhost`.
{% endhint %}

Once created, you should have all information about your app displayed in your profile:

<figure><img src="../../.gitbook/assets/Capture d’écran 2023-03-31 à 00.43.21.png" alt=""><figcaption><p>my Sismo Connect app</p></figcaption></figure>

The appId displayed on your app’s profile in the Factory is its unique identifier. You will use it to request proof from your users in your app’s frontend and to verify it in your contracts.

For this guide, you can use your own `appId`, such as `0x11b1de449c6c4adb0b5775b3868b28b3`.

### Select or create your group of users and get the groupId

The core concept of Sismo Connect is to enable users to selectively disclose statements to applications regarding pieces of personal data, that we call Data Shards, stored in their Data Vault. This is done through the use of zero-knowledge proofs. Data Shards originate from groups that can also be created in the Factory.

All groups created on the Sismo protocol are public, so if someone has already created a group that meets your needs, you can reuse it. You can explore all existing groups on our [**Factory explorer**](https://factory.sismo.io/groups-explorer). If you have very specific needs, you can create your own group using our [**no-code interface**](https://factory.sismo.io/groups-explorer) or by following this [**tutorial**](../sismo-hub/create-your-group.md) to submit a pull request to our repository.

For the zkDrop app, we will use the group with all users who contributed to The Merge named [`the-merge-contributor`](https://github.com/sismo-core/sismo-hub/blob/main/group-generators/generators/the-merge-contributor/index.ts). To find its unique group ID (groupId) , you can use the [group explorer in the Factory](https://factory.sismo.io/groups-explorer?search=the-mer). It’s better to try it out yourself, but you should ultimately find the group’s unique id: `0x42c768bb8ae79e4c5c05d3b51a4ec74a`

Congratulations! You now have an appId (`0x11b1de449c6c4adb0b5775b3868b28b3`) and a groupId (`0x42c768bb8ae79e4c5c05d3b51a4ec74a`), which means you can integrate [Sismo Connect](../../readme/sismo-connect.md) into your app. Let’s now focus on how to generate and verify the proof.

### Request data privately from your users

User proofs are generated in the [Data Vault App](../../what-is-sismo/data-vault.md) thanks to requests from your frontend. There are three types of requests that you can leverage:

* A **Claim request** will request a proof of group membership from your user. We will use it for zkDrop since we want our users to prove that they are contributors to the Merge (i.e. they are members of the group with id `0x42c768bb8ae79e4c5c05d3b51a4ec74a`).
* An **Auth request** will request a proof of account ownership from your user. In zkDrop, we use it to request a proof of a Data Vault ownership (i.e. a user prove that he owns a Data Vault on Sismo). This Auth request is useful to get an anonymized user identifier (userId).
* A **Message Signature request** will request to generate a proof that can't be verified without this specific message associated to it (that's why we call it a signature). In zkDrop, users will choose an address on which they wish to receive the airdrop and we will use this address to generate the proof.

To easily generate a proof for these three requests, you can use the [`@sismo-core/sismo-connect-react`](../../technical-documentation/sismo-connect/react.md) package or the [`@sismo-core/sismo-connect-client`](../../technical-documentation/sismo-connect/client.md) package.

The following snippets will showcase the React usage but feel free to see the [`@sismo-core/sismo-connect-client`](../../technical-documentation/sismo-connect/client.md) documentation for more vanilla javascript/typescript integration.

First, you will need to import the React package:

```bash
# go to the front folder
cd front

# install the react package
yarn add @sismo-core/sismo-connect-react
```

After importing, you will be able to use the Sismo Connect button in your app. By clicking on this button your user will be redirected to the [Data Vault App](https://docs.sismo.io/sismo-docs/technical-documentation/data-vault-app), to generate a proof for the specified group.

<figure><img src="../../.gitbook/assets/Capture d’écran 2023-04-12 à 11.30.56.png" alt=""><figcaption><p>Sismo Connect button</p></figcaption></figure>

To do so, you have to use the `SismoConnectButton` component, you can see below several examples one how to use the button and request different proofs from your user.

{% tabs %}
{% tab title="Claim / Vault Auth" %}
Here is the request we make in the frontend for the rest of the tutorial. You can see a more complex request on the other tab.

```typescript
import {
  SismoConnectButton,
  SismoConnectClientConfig,
  SismoConnectResponse,
  AuthType
} from "@sismo-core/sismo-connect-react";

import { ethers } from "ethers";

export const config: SismoConnectClientConfig = {
  appId: "0x11b1de449c6c4adb0b5775b3868b28b3" // appId you registered
};

 <SismoConnectButton
   // Use the config you defined above
    config={config}
    // request a proof of Data Vault Ownership
    auths={[{authType: AuthType.VAULT}]}
    // request a proof of group membership for `the-merge-contributor`
    claims={[{ groupId: "0x42c768bb8ae79e4c5c05d3b51a4ec74a" }]}
    // request to generate a proof that can't valid without 
    // a message chosen by the user
    signature={{
      // here we encode the message using ethers
      // we encode it as an address
      // we will use it to airdrop the ERC721 on this address in our contracts
      message: ethers.utils.defaultAbiCoder.encode(
       // you have to get the `destination` 
       // to add it in the messageSignatureRequest
       ["address"], [destination]) 
    }}
    onResponseBytes={(responseBytes: string) => {
     //Send the response to your contract to verify it
     //thanks to the @sismo-core/sismo-connect-solidity package
     //Will see how to do this in next part of this tutorial
    }}
/>
```

The `auths` field will request a proof of Data Vault ownership and the `claims` field will request a proof of group membership for the `the-merge-contributor` group.

The `signature` field is the destination address where the NFT will be send at the end.

{% hint style="info" %}
The `dataRequest` has more optional parameters available:

* **`groupTimestamp`**: This parameter specifies the timestamp of the group snapshot a user wants to prove membership in. By default, this timestamp is set to ‘latest’ to use the latest generated snapshot.
* **`value`**: In groups, every account is associated with a value, which can represent the number of tokens staked, the voting power of the account, etc. The `requestedValue` parameter allows you to specify the value your user needs to have in the group to generate the proof. By default, it is set to 1.
* **`claimType`**: The claimType can force users to prove that they have a value in the group greater than or equal (`ClaimType.GTE`) to the `requested value` or strictly equal (`ClaimType.EQ`)to the `requested value`. By default, it is `“GTE”.`\\

You can see the [documentation](../../technical-documentation/sismo-connect/client.md) if you want to learn more about this.
{% endhint %}
{% endtab %}

{% tab title="Claim Two Auths / Signature" %}
You can request optional proofs. It is up to the user to choose to share his data with your app or not. You can also make your users sign a message.

```typescript
import {
  SismoConnectButton,
  SismoConnectClientConfig,
  SismoConnectResponse,
  AuthType
} from "@sismo-core/sismo-connect-react";

import { ethers } from "ethers";

export const config: SismoConnectClientConfig = {
  appId: "0x0ad03e347304dd6c19d9aa75db8659d9" // appId you registered
};

 <SismoConnectButton
    config={config}
    auths={[
       // request a proof of Twitter account Ownership (optional)
       {authType: AuthType.TWITTER, isOptional: true},
       // request a proof of GitHub account Ownership (required)
       {authType: AuthType.GITHUB},
    ]}
    // request a proof of group membership for `sismo-contributors`
    claims={[{ groupId: "0xe9ed316946d3d98dfcd829a53ec9822e" }]}
    signature={{
       // here we encode the message using ethers
       // we encode it as an address
       // we will use it to airdrop the ERC721 on this address in our contracts
       message: ethers.utils.defaultAbiCoder.encode(
       // you have to get the `destination` 
       // to add it in the messageSignatureRequest
       ["address"], [destination]) 
    }}
    onResponseBytes={(responseBytes: string) => {
     //Send the response to your contract to verify it
     //thanks to the @sismo-core/sismo-connect-solidity package
     //Will see how to do this in next part of this tutorial
    }}
/>
```

The `auths` field will request a proof of Twitter account ownership (that is optional) and a proof of GitHub account ownership (that is required), the `claims` field will request a proof of group membership for the `the-merge-contributor` group and the `signature` field is the destination address where the NFT will be send at the end.
{% endtab %}
{% endtabs %}

{% hint style="success" %}
If you are not part of the group, feel free to use the optional `devMode` in your sismoConnectConfig. Set the boolean field enabled as true and input a list of groups that will be used instead of the actual Sismo groups. Beware that this devMode should only be used when prototyping, if you keep such config in production, these groups will be able to make proofs in their Vaults.
{% endhint %}

```typescript
import {
  SismoConnectButton,
  SismoConnectClientConfig,
  SismoConnectResponse,
  AuthType
} from "@sismo-core/sismo-connect-react";

import { ethers } from "ethers";

const config: SismoConnectClientConfig = {
    appId: "0x11b1de449c6c4adb0b5775b3868b28b3", // appId you registered
    devMode: {
      // will use the Dev Sismo Data Vault https://dev.vault-beta.sismo.io/
      enabled: true, 
      // overrides a group with these addresses
      devGroups: [{
        groupId: "0x42c768bb8ae79e4c5c05d3b51a4ec74a",
        data: {
          "0x123...abc": 1, 
          "0x456...efa": 2
        },
      }]
    }
};

// you can use the button as before
```

After your user generates the proof, he will be automatically redirected back to your app.

After your user generates the proof, he will be automatically redirected back to your app. The `onResponseBytes` props will allow you to get the response containing the proof.

To get the response of your user after his redirection without using the `SismoConnectButton` you can also use the `useSismoConnect` hook. This allows you to redirect your user to a page other than the page with the button integrated.

The `useSismoConnect` will get the response back as a typescript object and as bytes. The typescript object can be used to see the response in clear while the responseBytes is the [`sismoConnectResponse`](../../technical-documentation/sismo-connect/solidity-library.md) encoded as bytes that you will send to your smart contracts.

```typescript
const { response, responseBytes } = useSismoConnect({config: sismoConnectConfig });
```

You can find the sismoConnectButton already implemented here: `front/src/pages/MergooorPass/index.tsx`

If you want, you can change the `appId` by yours, but also add your own address in the dev mode so you can use `the-merge-contributor` group (already configured).

Feel free to configure the button as you wish!

{% hint style="success" %}
You can find all the frontend code snippets in the [boilerplate repository](https://github.com/sismo-core/sismo-connect-boilerplate/tree/main/src/pages/on-chain).
{% endhint %}

Well done! You’re halfway there! 💪

Now that the frontend is done, let's jump to the contracts!

As you noticed, you now have the proof as bytes and you will send this proof in your contracts to check its validity with respect to `the-merge-contributor` group, the Data Vault ownership requested and the message signed by the user.

### Verify the proof on-chain and gate your contracts

```bash
curl -L https://foundry.paradigm.xyz | bash
# follow the command ouput intructions
# then open a new terminal to be able to use forge command later
```

The first thing to setup is to make your contract inherit from Sismo Connect so that it can pass the appId in the constructor to configure Sismo Connect for your app (it is similar to the sismoConnectConfig we have in our frontend).

We also pass the groupId in the constructor to have it as an immutable in the contract.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

// we can use Sismo Connect thanks to this import
import "sismo-connect-solidity/SismoLib.sol"; 
import {ERC721} from "@openzeppelin/contracts/token/ERC721/ERC721.sol";

// Your contract inherits from SismoConnect
contract ZKDropERC721 is ERC721, SismoConnect {
  // the group we want our users to be member of
  bytes16 public immutable GROUP_ID; 
  
  ...
  
  constructor(
    string memory name,
    string memory symbol,
    string memory baseTokenURI,
    bytes16 appId,  // <- the appId registered in the Factory
    bytes16 groupId // <- the groupId of the-merge-contributor group
  ) ERC721(name, symbol) SismoConnect(appId) {
    GROUP_ID = groupId;
    _setBaseTokenURI(baseTokenURI);
  }

  ...
  
}
```

{% hint style="info" %}
You can find the contract here `src/ZKDropERC721.sol`
{% endhint %}

You can now start to implement new functions that take a `SismoConnectResponse` of type `bytes`. It will allow you to send the `responseBytes` that you received in your frontend to your contracts. For example, here is the function that will be used for claiming the ERC721 by users.

```solidity
// the function takes the response as bytes 
// and the `to` address on which the user wants to claim the ERC721
function claimWithSismoConnect(bytes memory response, address to) public {
    // it verifies the response with respect to an Auth, a Claim and a Signature
    // and returns a SismoConnectVerifiedResult
    SismoConnectVerifiedResult memory result = verify({
        // the response as bytes
        responseBytes: response,
        // we recreate the Auth made earlier by the front 
        // to be sure that the proof is valid with respect to a Data Vault ownership
        auth: buildAuth({authType: AuthType.VAULT}),
        // we recreate the Claim made earlier by the front 
        // to be sure that the proof is valid with respect to this group membership
        claim: buildClaim({groupId: GROUP_ID}),
        // we recreate the message signature request 
        // to be sure that the proof is made for this address
        signature: buildSignature({message: abi.encode(to)})
    });
    
    // we use the userId requested with the Auth request 
    // as the tokenId of the ERC721 to ensure that the same vault
    // can't claim to different ERC721 token
    uint256 tokenId = result.getUserId(AuthType.VAULT);

    // we mint the ERC721 to the address
    _mint(to, tokenId);
  }
```

This function should give you a huge feeling on how to leverage the three type of requests available: the Claim request, the Auth request and the Signed Message request. By using them, you can prove a user group membership, an account ownership while making the user sign a message in his Data Vault.\
\
By calling the `claimWithSismoConnect` function with the `sismoConnectResponse` as bytes received by your front, you have now the full flow of an on-chain Sismo Connect integration. 💪

{% hint style="success" %}
We highly encourage you to take a look at the different on-chain boilerplates showcased in this [repository](https://github.com/sismo-core/sismo-connect-boilerplate). You can see simple contracts gating a counter with a sismoConnectResponse and the tests associated to it.
{% endhint %}

### Test your contracts

While following this tutorial, you obviously thought about testing your contracts before deploying them. To test them, you will need some custom proofs that can be generated easily with a script.\
The aim of the script is to mock your frontend client.

```typescript
const opn = require('better-opn')
import {
  AuthType,
  SismoConnect,
  SismoConnectClientConfig,
} from '@sismo-core/sismo-connect-client'
import { ethers } from 'ethers'

// This is the configuration needed to
// use the appId registered in the Sismo Factory
// and configure devMode if needed
const sismoConnect = SismoConnect({
  appId: '0x112a692a2005259c25f6094161007967',
  devMode: {
    // you need to enable this to get the response as bytes for your test
    displayRawResponse: true,
  },
} as SismoConnectClientConfig)

const abiCoder = new ethers.AbiCoder()

const url = sismoConnect.getRequestLink({
  claims: [{groupId: '0xe9ed316946d3d98dfcd829a53ec9822e'}],
  auths: [{authType: AuthType.VAULT}],
  signature: {
    message: abiCoder.encode(
      ['address'],
      ['0x7def1d6D28D6bDa49E69fa89aD75d160BEcBa3AE']
    ),
  },
})

opn(url)
```

You can then call this script by using:

```bash
# at the root of the repo
npx ts-node generate-proof.ts
```

{% hint style="warning" %}
Don't forget to apply the same configuration to the test that you used for the `sismoConnectButton` of the front before.
{% endhint %}

You will be provided a `SismoConnectResponse` of type `bytes` in a modal by completing the flow in the Data Vault app.

<figure><img src="../../.gitbook/assets/Capture d’écran 2023-04-15 à 22.27.23.png" alt=""><figcaption><p>The sismoConnectResponse as bytes</p></figcaption></figure>

Then we need to create a contract that will use the `ZKDropERC721` contract we created, to test our verification process on-chain. You can find the already implemented contract here: `test/ZKDropERC721.t.sol`

{% hint style="warning" %}
Don't forget to copy paste the response above into the test in the `sismoConnectResponseEncoded` variable, and the Registry Tree Root in the `_registerTreeRoot` variable.

You can also define the address where you will drop the NFT in the `user` variable.
{% endhint %}

{% code overflow="wrap" %}
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.13;

import "forge-std/Test.sol";
import "forge-std/console.sol";
import {ZKDropERC721} from "src/ZKDropERC721.sol";
import "sismo-connect-solidity/SismoLib.sol";
import {SismoConnectBaseTest} from "./SismoConnectBaseTest.t.sol";

contract ZKDropERC721Test is SismoConnectBaseTest {
    bytes16 immutable APP_ID = 0x11b1de449c6c4adb0b5775b3868b28b3;
    bytes16 immutable ZK = 0xe9ed316946d3d98dfcd829a53ec9822e;
    address user = 0x040200040600000201150028570102001e030E26;
    ZKDropERC721 zkdrop;

    address immutable addressesProviderOwner = 0xaee4acd5c4Bf516330ca8fe11B07206fC6709294;

    function setUp() public {
        zkdrop = new ZKDropERC721({
            appId: APP_ID,
            groupId: ZK,
            name: "ZKDrop test",
            symbol: "test",
            baseTokenURI: "https://test.com"
        });

        _registerTreeRoot(0x254bfcf1f638e959e511d98b551930eba311d6855d17b2d0337b232a56dd1ab7);
    }

    function testClaimAndAuthWithSignedMessage() public {
        bytes
            memory zkConnectResponseEncoded = hex"000000000000000000000000000000000000000000000000000000000000002011b1de449c6c4adb0b5775b3868b28b300000000000000000000000000000000b8e2054f8a912367e38a22ce773328ff000000000000000000000000000000007369736d6f2d636f6e6e6563742d76310000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000a000000000000000000000000000000000000000000000000000000000000000e00000000000000000000000000000000000000000000000000000000000000020000000000000000000000000040200040600000201150028570102001e030e2600000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000040000000000000000000000000000000000000000000000000000000000000052000000000000000000000000000000000000000000000000000000000000000a000000000000000000000000000000000000000000000000000000000000000c068796472612d73322e310000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000001e000000000000000000000000000000000000000000000000000000000000004c00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000100000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000000e9ed316946d3d98dfcd829a53ec9822e000000000000000000000000000000006c617465737400000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000100000000000000000000000000000000000000000000000000000000000000c0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000002c028443b3c9adba5091b208610b9554edfcbe5466d3bf00b38f39117f349af75a711c11e8c8dba4e3871884ac3fac57dcdbdb9d8f10b42a7e790c2a6856316387709fab377e567bf665d0138a05cbd4bf75d8656f9c1f78cdf612ec012ca34690b1670339c6599d3509fd00748d1bff5626879b9271d10e22f756faaaf7cb1e5ca183dfcb7587c34ec2349f5a2e5ab471f65b23747b7758d5ad0bbd6aa6e1b6fb025928d4437c5cdbc226ceed8e3b84711f73eb1eeb20e9262c60dbacc11c62a2220acdabce6ad3e189f98cfd36662e9e5e8926b53468146c57aadb8b06c9177a0006681152660c6f63d68bbe700070d7bcfcf1d869daf69b47d7005ead70c7aa600000000000000000000000000000000000000000000000000000000000000001e762fcc1e79cf55469b1e6ada7c8f80734bc7484f73098f3168be945a2c00842ab71fb864979b71106135acfa84afc1d756cda74f8f258896f896b4864f025630423b4c502f1cd4179a425723bf1e15c843733af2ecdee9aef6a0451ef2db74254bfcf1f638e959e511d98b551930eba311d6855d17b2d0337b232a56dd1ab704f81599b826fa9b715033e76e5b2fdda881352a9b61360022e30ee33ddccad902fd4824b3d910fd2ba914af781e67453df1996c7bf136231daa63b33f37f8220000000000000000000000000000000000000000000000000000000000000001285bf79dc20d58e71b9712cb38c420b9cb91d3438c8e3dbaf07829b03ffffffc000000000000000000000000000000000000000000000000000000000000000008e09419bd628d6f67fc72f1c3d31f7053eb8ddc56a41b87badbfdfbe2ef063b01935d4d418952220ae0332606f61de2894d8b84c3076e6097ef3746a1ba04a500000000000000000000000000000000000000000000000000000000000000010000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000a000000000000000000000000000000000000000000000000000000000000001a068796472612d73322e310000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000001c000000000000000000000000000000000000000000000000000000000000004a00000000000000000000000000000000000000000000000000000000000000001000000000000000000000000000000000000000000000000000000000000002000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000108e09419bd628d6f67fc72f1c3d31f7053eb8ddc56a41b87badbfdfbe2ef063b00000000000000000000000000000000000000000000000000000000000000a00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000002c025bdf36fcf54e92b6662f6aee580df03d53940f55f113f2ad2ae4de3da7c116f134e3244eccbcd6e72abecf792ff5b526151cb1df71656b71739c2186c0ef9791995cc57cb2cec80fb6796138ab9ed680384c543b5d128eaac4fa7cc494bcd0f0c9e758740c9371ffa2b28adddebe2d46e4fc2da06e4304d0446eb1c68e4dad0134239d2be3fdacbdefbf85d26b27d55b5b777798404a43f69dfb37de8aa2bba02a3309430fbabd6055cd7d5d69430ab278e0655d181c45bc742b7d7f09649a0203b9fb16228b273feee8ce62c42fccfe4d86a57a02c73c8e61b041591034e8a02603afe2f129a765fd876887d559e008fa31b208e1da1407da43ea6abb02bab00000000000000000000000000000000000000000000000000000000000000001e762fcc1e79cf55469b1e6ada7c8f80734bc7484f73098f3168be945a2c00842ab71fb864979b71106135acfa84afc1d756cda74f8f258896f896b4864f025630423b4c502f1cd4179a425723bf1e15c843733af2ecdee9aef6a0451ef2db7400000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000008e09419bd628d6f67fc72f1c3d31f7053eb8ddc56a41b87badbfdfbe2ef063b01935d4d418952220ae0332606f61de2894d8b84c3076e6097ef3746a1ba04a5000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000";
        zkdrop.claimWithSismoConnect(zkConnectResponseEncoded, user);
    }

    function getEdDSAPubKey() public pure returns (uint256[2] memory) {
        return [
            0x7f6c5612eb579788478789deccb06cf0eb168e457eea490af754922939ebdb9,
            0x20706798455f90ed993f8dac8075fc1538738a25f0c928da905c0dffd81869fa
        ];
    }

    function getRoot() public pure returns (uint256) {
        return 0x1d4a72bd1c1e4f9ab68c3c4c55afd3e582685a18b9ec09fc96136619d2513fe8;
    }
}
```
{% endcode %}

You can now test your contract by doing a fork test on the Goerli network for example. You will need a fork test because the Sismo Connect Solidity library interacts with a contract called `SismoConnectVerifier` already deployed on chain.

You can now run the following command to launch the test:

```bash
forge test -vvvv --fork-url https://rpc.ankr.com/eth_goerli

# you can aslo use the rpc url you want by passing an environment variable
forge test -vvvv --fork-url $RPC_URL
```

And... 🥁 it's over! You just checked onchain that the proof was valid and therefore airdropped an NFT to the defined `user` 🙌\
\
If you have any questions about integrating sismoConnect, don’t hesitate to reach out. The team will be happy to answer any questions you may have.

Get involved in the Sismo community:

* Look out for [**hackathons**](https://www.notion.so/sismo/Sismo-x-ETHPorto-2023-cbda827ea5f2469aa5fdbb4955fc18d6?pvs=4) that we are participating in
* Join our [**Discord**](https://discord.gg/sismo) or our [**Dev Telegram**](https://t.me/+Z-SwcvXZFRVhZTQ0)
* See the [**Sismo-hub contributing page**](https://github.com/sismo-core/sismo-hub/issues)
