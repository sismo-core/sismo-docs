# Gate your contracts with Sismo Connect (Advanced)

### What’s inside?

This tutorial will walk you through how we built zkDrop, an application allowing whitelisted wallets and GitHub accounts to benefit from an airdrop on a public address while keeping their eligible account private thanks to an on-chain [**Sismo Connect**](../../readme/sismo-connect.md) integration.

To get a feel of what you will build in this tutorial, you can try out a demo [**here**](https://demo.zkdrop.io/mergooor-pass).

{% hint style="info" %}
You can access the open-source repository of the demo (in Next) [here](https://github.com/sismo-core/zkdrop).

The repository use the [`@sismo-core/sismo-connect-react`](https://docs.sismo.io/sismo-docs/technical-documentation/zkconnect/zkconnect-react-request) package to request the proof and the [Sismo Connect Solidity library](../../technical-documentation/sismo-connect/solidity-library.md) to verify it.
{% endhint %}

### Tutorial use case

We will use the example of [The Merge](https://ethereum.org/en/upgrades/merge/) contributors and [the initiative of Tim Beiko](https://twitter.com/zkdrop\_io/status/1615716498000977921?s=20) to airdrop to contributors to The Merge a ZK Soulbound token (here an ERC721) to allow them to join events hosted by [ETHGlobal](https://ethglobal.com/), [EthCC](https://www.ethcc.io/), [Edcon](https://www.edcon.io/) and [Devcon](https://devcon.org/).

For anonymous contributors to The Merge, we will build zkDrop that enables them to claim the airdrop on a public address in a privacy-preserving manner. These anonymous contributors will then be able to join events with this ERC721.

{% hint style="info" %}
We definitely recommend our entry-point tutorial for a quick off-chain Sismo Connect integration [here](authenticate-your-users-with-sismo-connect.md). Feel free to take a look or even try it first before following this more advanced one.
{% endhint %}

### Choose your stack ([Next.js](https://nextjs.org/) and [Foundry](https://book.getfoundry.sh/) recommended)

To implement sismoConnect, you'll need both a frontend and some smart contracts using [Foundry](https://book.getfoundry.sh/). The frontend will request the proof, and the smart contracts will verify it on-chain thanks to the Sismo Connect Solidity library.

For this tutorial, we recommend using the Next.js stack for your application, which is a full-stack React framework. Next.js also offers a deployment service called Vercel, which makes it easy to deploy your app in just two clicks.

To create a new Next.js project, run the following command:

```
yarn create next-app --typescript
```

That's it! Your frontend will be located in `src/pages/index.tsx`. We will handle the installation of Foundry in the solidity part of the tutorial.

You can find the complete Next.js boilerplate repository setup with Sismo Connect [here](https://github.com/sismo-core/sismo-connect-boilerplate).

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

<figure><img src="../../.gitbook/assets/Capture d’écran 2023-03-14 à 19.47.52 (1) (1) (1) (1).png" alt=""><figcaption><p>Register your Sismo Connect App in the <a href="https://factory.sismo.io/apps-explorer">Sismo Factory</a></p></figcaption></figure>

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

{% hint style="success" %}
You can find all the frontend code snippets in the [boilerplate repository](https://github.com/sismo-core/sismo-connect-boilerplate/tree/main/src/pages/on-chain).
{% endhint %}

Well done! You’re halfway there! 💪

Now that the frontend is done, let's jump to the contracts!

As you noticed, you now have the proof as bytes and you will send this proof in your contracts to check its validity with respect to `the-merge-contributor` group, the Data Vault ownership requested and the message signed by the user.

### Verify the proof on-chain and gate your contracts

To be able to use the [Sismo Connect Solidity Library](../../technical-documentation/sismo-connect/solidity-library.md), you will first need to install Foundry.

```bash
curl -L https://foundry.paradigm.xyz | bash
```

{% hint style="info" %}
Here is the link to the Foundry book if you are not familiar with it: [https://book.getfoundry.sh/](https://book.getfoundry.sh/)
{% endhint %}

{% hint style="success" %}
We HIGHLY recommend to fork our`contracts` folder of zkDrop to be well setup : [https://github.com/sismo-core/zkdrop/tree/main/contracts](https://github.com/sismo-core/zkdrop/tree/main/contracts)\
You can also fork the boilerplate repository: [https://github.com/sismo-core/sismo-connect-boilerplate](https://github.com/sismo-core/sismo-connect-boilerplate)
{% endhint %}

To fork our contracts folder, you can just clone the zkDrop repository on your machine and copy it to your project.

```bash
# clone the zkDrop repository in your home folder for example
git clone https://github.com/sismo-core/zkdrop.git

# copy the contracts folder in your project
cd zkdrop
cp -r ./contracts <your-project-path>
```

You can then run the following command to install all the needed dependencies in your project.

<pre class="language-bash"><code class="lang-bash"><strong># go back to your project
</strong><strong>cd &#x3C;your-project-path>
</strong><strong>cd contracts
</strong>forge install
</code></pre>

You can now put your contracts under `contracts/src` and start to use the [Sismo Connect Solidity Library](../../technical-documentation/sismo-connect/solidity-library.md).

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
  ClaimType,
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
npx ts-node <script-name>.ts
```

You will be provided a `SismoConnectResponse` of type `bytes` in a modal by completing the flow in the Data Vault app. You can choose to copy paste this response in your tests.

<figure><img src="../../.gitbook/assets/Capture d’écran 2023-03-31 à 16.23.36.png" alt=""><figcaption><p>The sismoConnectResponse as bytes</p></figcaption></figure>

You can test your contract by doing a fork test on the Goerli network for example. You will need a fork test because the Sismo Connect Solidity library interacts with a contract called `SismoConnectVerifier` already deployed on chain. You can copy paste the following test file in `contracts/test/ZkDropERC721.t.sol` and launch the following command:

```bash
forge test -vvvv --fork-url https://rpc.ankr.com/eth_goerli

# you can aslo use the rpc url you want by passing an environment variable
forge test -vvvv --fork-url $RPC_URL
```

{% code overflow="wrap" %}
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.13;

import "forge-std/Test.sol";
import "forge-std/console.sol";
import {ZKDropERC721} from "src/ZKDropERC721.sol";
import {CommitmentMapperRegistryMock} from "test/mocks/CommitmentMapperRegistryMock.sol";
import {AvailableRootsRegistryMock} from "test/mocks/AvailableRootsRegistryMock.sol";
import "sismo-connect-solidity/SismoLib.sol";

contract ZKDropERC721Test is Test {
    bytes16 immutable APP_ID = 0x11b1de449c6c4adb0b5775b3868b28b3;
    bytes16 immutable ZK = 0xe9ed316946d3d98dfcd829a53ec9822e;
    address user = 0x7def1d6D28D6bDa49E69fa89aD75d160BEcBa3AE;
    ZKDropERC721 zkdrop;

    address immutable addressesProviderOwner = 0xaee4acd5c4Bf516330ca8fe11B07206fC6709294;

    function setUp() public {

        zkdrop =
        new ZKDropERC721({appId: APP_ID, groupId: ZK, name: "ZKDrop test", symbol: "test", baseTokenURI: "https://test.com"});
        console.log("ZkDrop contract deployed at", address(zkdrop));

        vm.startPrank(0xa687922C4bf2eB22297FdF89156B49eD3727618b);
        address(0xF3dAc93c85e92cab8f811b3A3cCaCB93140D9304).call(
            abi.encodeWithSignature(
                "registerRoot(uint256)", 0xa51e98a112191871021d36d20b4812488328eba74a8a23788893026af69af36
                )
        );
        vm.stopPrank();
    }

    function testClaimAndAuthWithSignedMessage() public {
        bytes memory sismoConnectResponseEncoded =
            hex"000000000000000000000000000000000000000000000000000000000000002011b1de449c6c4adb0b5775b3868b28b300000000000000000000000000000000b8e2054f8a912367e38a22ce773328ff000000000000000000000000000000007a6b2d636f6e6e6563742d76320000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000800000000000000000000000000000000000000000000000000000000000000001000000000000000000000000000000000000000000000000000000000000002000000000000000000000000000000000000000000000000000000000000000c00000000000000000000000000000000000000000000000000000000000000180000000000000000000000000000000000000000000000000000000000000022068796472612d73322e310000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000002600000000000000000000000000000000000000000000000000000000000000540e9ed316946d3d98dfcd829a53ec9822e000000000000000000000000000000006c617465737400000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000001000000000000000000000000000000000000000000000000000000000000000100000000000000000000000000000000000000000000000000000000000000a000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000001000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000200000000000000000000000007def1d6d28d6bda49e69fa89ad75d160becba3ae00000000000000000000000000000000000000000000000000000000000002c00b64f961ab740d338d660cc4138c79225e8ecccb78a59dc52311169b6a00f70f27fb5a894b3c5344412bb00a63d13170c6dae480c4459fd41b3354f72064d11116b14847d2b64d7596286adaa13c3353bac911fc5fe0ac183b1775fc94b05218210b28d8fae8cc2bb2ea6d3a46cade59f01cafac0020ef38a21f3811279fb55f02e4a037e75c6a6b6b25ecf2979f65df7524cc0b2ab20d4270afb7610f1c49ae2372edacdf9eeb686cb4eee42490c1f918f4246fc4606b0d599e12bdacd20345249c36d3e6aeee76be3aaed8cd1f3d781ef91641f671cc9f44e3bda10643bd0d24b30bb0ed6d10e0663b8b1d6a4eeda5a868d54f63faa951f2ef159ce27556dd000000000000000000000000000000000000000000000000000000000000000009f60d972df499264335faccfc437669a97ea2b6c97a1a7ddf3f0105eda34b1d2ab71fb864979b71106135acfa84afc1d756cda74f8f258896f896b4864f025630423b4c502f1cd4179a425723bf1e15c843733af2ecdee9aef6a0451ef2db740a51e98a112191871021d36d20b4812488328eba74a8a23788893026af69af3604f81599b826fa9b715033e76e5b2fdda881352a9b61360022e30ee33ddccad90744e9b92802056c722ac4b31612e1b1de544d5b99481386b162a0b59862e0850000000000000000000000000000000000000000000000000000000000000001285bf79dc20d58e71b9712cb38c420b9cb91d3438c8e3dbaf07829b03ffffffc0000000000000000000000000000000000000000000000000000000000000000174c0f7d68550e40962c4ae6db9b04940288cb4aeede625dd8a9b0964939cdeb0000000000000000000000000000000011b1de449c6c4adb0b5775b3868b28b3000000000000000000000000000000000000000000000000000000000000000100000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000";
        zkdrop.claimWithSismoConnect(sismoConnectResponseEncoded, user);
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

Here it is!\
\
If you have any questions about integrating sismoConnect, don’t hesitate to reach out. The team will be happy to answer any questions you may have.

Get involved in the Sismo community:

* Look out for [**hackathons**](https://www.notion.so/sismo/Sismo-x-ETHPorto-2023-cbda827ea5f2469aa5fdbb4955fc18d6?pvs=4) that we are participating in
* Join our [**Discord**](https://discord.gg/sismo) or our [**Dev Telegram**](https://t.me/+Z-SwcvXZFRVhZTQ0)
* See the [**Sismo-hub contributing page**](https://github.com/sismo-core/sismo-hub/issues)
