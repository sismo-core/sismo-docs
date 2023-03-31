# Gate your contracts with zkConnect (Advanced)

### What’s inside?

This tutorial will walk you through how we built zkDrop, an application allowing whitelisted wallets and GitHub accounts to benefit from an airdrop on a public address while keeping their eligible account private thanks to an on-chain [**zkConnect**](../../what-is-sismo/zkconnect.md) integration.

To get a feel of what you will build in this tutorial, you can try out a demo [**here**](https://demo.zkdrop.io/mergooor-pass).

{% hint style="info" %}
You can access the open-source repository of the demo (in Next) [here](https://github.com/sismo-core/zkdrop).

The repository use the [`@sismo-core/zk-connect-react`](https://docs.sismo.io/sismo-docs/technical-documentation/zkconnect/zkconnect-react-request) package to request the proof and the [zkConnect Solidity library](../../technical-documentation/zkconnect/zkconnect-solidity-library-verify-on-chain-soon.md) to verify it.
{% endhint %}

### Tutorial use case

We will use the example of [The Merge](https://ethereum.org/en/upgrades/merge/) contributors and [the initiative of Tim Beiko](https://twitter.com/zkdrop\_io/status/1615716498000977921?s=20) to airdrop to contributors to The Merge a ZK Soulbound token (here an ERC721) to allow them to join events hosted by [ETHGlobal](https://ethglobal.com/), [EthCC](https://www.ethcc.io/), [Edcon](https://www.edcon.io/) and [Devcon](https://devcon.org/).&#x20;

For anonymous contributors to The Merge, we will build zkDrop that enables them to claim the airdrop on a public address in a privacy-preserving manner. These anonymous contributors will then be able to join events with this ERC721.

{% hint style="info" %}
We definitely recommend our entry-point tutorial for a quick off-chain zkConnect integration [here](authenticate-your-users-with-zkconnect.md). Feel free to take a look or even try it first before following this more advanced one.&#x20;
{% endhint %}

### Choose your stack ([Next.js](https://nextjs.org/) and [Foundry](https://book.getfoundry.sh/) recommended)

To implement zkConnect, you'll need both a frontend and some smart contracts using [Foundry](https://book.getfoundry.sh/). The frontend will request the proof, and the smart contracts will verify it on-chain thanks to the zkConnect Solidity library.

For this tutorial, we recommend using the Next.js stack for your application, which is a full-stack React framework. Next.js also offers a deployment service called Vercel, which makes it easy to deploy your app in just two clicks.&#x20;

To create a new Next.js project, run the following command:

```
yarn create next-app --typescript
```

That's it! Your frontend will be located in `src/pages/index.tsx`. We will handle the installation of Foundry in the solidity part of the tutorial.

You can find the complete example of a Next.js repository setup with zkConnect [here](https://github.com/sismo-core/zkdrop).

### Register your zkConnect App in the Factory

Before you begin integrating [**zkConnect**](../../what-is-sismo/zkconnect.md), you must create first a zkConnect app in the [**Sismo Factory**](https://factory.sismo.io/apps-explorer). This step is mandatory to obtain an application Id (`appId`), which is required during the zkConnect development process.

<details>

<summary>Why is an <code>appId</code> mandatory for zkConnect?</summary>

The `appId` will be used to compute the vault identifier which is the the unique identifier for a user creating a proof for your app. The vault identifier is simply the hash of the user vault secret and the appId.

$$vaultId = hash(vaultSecret, appId)$$

If we had removed the appId from this simple calculus, we would have had the same vaultIdentifier for the same vaultSecret, effectively leaking information about a user that uses zkConnect on two different apps. The vaultId would just be the same between the apps and the user can be tracked if vault identifiers become public.

But if we introduce an appId, the vaultId is now different between apps and the same user will have two different vault ids between two different apps, effectively preserving the user privacy this time. 🤘&#x20;

You can learn more about this notion in the [Vault & Proof identifiers article](../../technical-concepts/vault-and-proof-identifiers.md).

</details>

<figure><img src="../../.gitbook/assets/Capture d’écran 2023-03-14 à 19.47.52 (2).png" alt=""><figcaption><p>Register your zkConnect App in the <a href="https://factory.sismo.io/apps-explorer">Sismo Factory</a></p></figcaption></figure>

You can register a zkConnect app here: [https://factory.sismo.io/apps-explorer](https://factory.sismo.io/apps-explorer).\
\
To create a zkConnect app, you need to log in with Sign-In With Ethereum and click on “create a new zkConnect app”. You will need to register an App Name, enter a description, and upload a logo alongside registering authorized domains. Pay attention to authorized domains, as these are the urls where the appId that will be created can be used for [zkConnect](../../technical-documentation/zkconnect/).&#x20;

{% hint style="info" %}
Feel free to add `*.com` to authorized domains when following along this tutorial. This will allow to whitelist `localhost`.
{% endhint %}

Once created, you should have all information about your app displayed in your profile:

<figure><img src="../../.gitbook/assets/Capture d’écran 2023-03-31 à 00.43.21.png" alt=""><figcaption><p>my zkConnect app</p></figcaption></figure>

The appId displayed on your app’s profile in the Factory is its unique identifier. You will use it to request proof from your users in your app’s frontend and to verify it in your contracts.

For this guide, you can use your own `appId`, such as `0x11b1de449c6c4adb0b5775b3868b28b3`.

### Select or create your group of users and get the groupId

The core concept of ZK Connect is to enable users to selectively disclose statements to applications regarding pieces of personal data, that we call Data Shards, stored in their Data Vault. This is done through the use of zero-knowledge proofs. Data Shards originate from groups that can also be created in the Factory.

All groups created on the Sismo protocol are public, so if someone has already created a group that meets your needs, you can reuse it. You can explore all existing groups on our [**Factory explorer**](https://factory.sismo.io/groups-explorer). If you have very specific needs, you can create your own group using our [**no-code interface**](https://factory.sismo.io/groups-explorer) or by following this [**tutorial**](../sismo-hub/create-your-group.md) to submit a pull request to our repository.

For the zkDrop app, we will use the group with all users who contributed to The Merge named [`the-merge-contributor`](https://github.com/sismo-core/sismo-hub/blob/main/group-generators/generators/the-merge-contributor/index.ts). To find its unique group ID (groupId) , you can use the [group explorer in the Factory](https://factory.sismo.io/groups-explorer?search=the-mer). It’s better to try it out yourself, but you should ultimately find the group’s unique id: `0x42c768bb8ae79e4c5c05d3b51a4ec74a`

Congratulations! You now have an appId (`0x11b1de449c6c4adb0b5775b3868b28b3`) and a groupId (`0x42c768bb8ae79e4c5c05d3b51a4ec74a`), which means you can integrate [zkConnect](../../what-is-sismo/zkconnect.md) into your app. Let’s now focus on how to generate and verify the proof.

### Request data privately from your users

User proofs are generated in the [Data Vault App](../../what-is-sismo/data-vault.md) thanks to requests from your frontend. There are three types of requests that you can leverage:

* A **Claim request** will request a proof of group membership from your user. We will use it for zkDrop since we want our users to prove that they are contributors to the Merge (i.e. they are members of the group with id `0x42c768bb8ae79e4c5c05d3b51a4ec74a`).
* An **Auth request** will request a proof of account ownership from your user. In zkDrop, we use it to request a proof of a Data Vault ownership (i.e. a user prove that he owns a Data Vault on Sismo). This Auth request is useful to get an anonymized user identifier (userId).
* A **Message Signature request** will request to generate a proof that can't be verified without this specific message associated to it (that's why we call it a signature). In zkDrop, users will choose an address on which they wish to receive the airdrop and we will use this address to generate the proof.

To easily generate a proof for these three requests, you can use the [`@sismo-core/zk-connect-react`](../../technical-documentation/zkconnect/zkconnect-react-request.md) package or the [@sismo-core/zk-connect-client](../../technical-documentation/zkconnect/zkconnect-client-request.md) package.

{% tabs %}
{% tab title="React / Next.js" %}
First, you will need to import the following package in your application:

```bash
yarn add @sismo-core/zk-connect-react
```

\
After importing, you will be able to use the zkConnect button in your app.

<figure><img src="../../.gitbook/assets/zkConnect (2).png" alt=""><figcaption><p>zkConnect button</p></figcaption></figure>



To do so, you will first have to create a `zkConnectConfig` with the appId you registered in the Factory.

<pre class="language-typescript"><code class="lang-typescript">import { AuthType, useZkConnect, ZkConnectButton, ZkConnectClientConfig } from "@sismo-core/zk-connect-react";

<strong>export const zkConnectConfig: ZkConnectClientConfig = {
</strong>  appId: "0x11b1de449c6c4adb0b5775b3868b28b3"
}
</code></pre>



You can then use the `ZkConnectButton` component with this zkConnectConfig and the requests you want proof from. By clicking on this button your user will be redirected to the [Data Vault App](https://docs.sismo.io/sismo-docs/technical-documentation/data-vault-app) to generate a proof for the specified groupId, the Data Vault ownership and the signed message requested.&#x20;

NB: We make three requests but we receive one proof that has been generated with these three requests.

```typescript
import { AuthType, useZkConnect, ZkConnectClientConfig, ZkConnectButton, ZkConnectResponse } from "@sismo-core/zk-connect-react";
import { ethers } from "ethers";

export const zkConnectConfig: ZkConnectClientConfig = {
  appId: "0x11b1de449c6c4adb0b5775b3868b28b3" // zkDrop appId I created
}

<ZkConnectButton 
  // Use the config you defined above
  config={zkConnectConfig}
  // request a group membership 
  claimRequest={{
    // groupId of the-merge-contributor group
    groupId: "0x42c768bb8ae79e4c5c05d3b51a4ec74a"
  }}
  // request an account ownership
  authRequest={{
    // we specify that the we want a proof of Data Vault ownership
    authType: AuthType.ANON
  }}
  // request to generate a proof that can't valid without 
  // a message chosen by the user
  messageSignatureRequest={
    // here we encode the message using ethers
    // we encode it as an address
    // we will use it to airdrop the ERC721 on this address in our contracts
    ethers.utils.defaultAbiCoder.encode(
     // you have to get the `destination` 
     // to add it in the messageSignatureRequest
     ["address"], [destination]) 
  }
/>
```



{% hint style="info" %}
The `claimRequest` has more optional parameters available:

* **`groupTimestamp`**: This parameter specifies the timestamp of the group snapshot a user wants to prove membership in. By default, this timestamp is set to ‘latest’ to use the latest generated snapshot.
* **`requestedValue`**: In groups, every account is associated with a value, which can represent the number of tokens staked, the voting power of the account, etc. The `requestedValue` parameter allows you to specify the value your user needs to have in the group to generate the proof. By default, it is set to 1.
* **`comparator`**: The comparator can force users to prove that they have a value in the group greater than or equal (`”GTE”`) to the `requestedValue` or strictly equal (`”EQ”`)to the `requestedValue`. By default, it is `“GTE”.`\
  ``

You can see the [documentation](../../technical-documentation/zkconnect/zkconnect-client-request.md) if you want to learn more about Claim requests. You can also learn more about Auth requests that can be requests of Data Vault ownership but only Twitter or GitHub account ownership for example.
{% endhint %}



{% hint style="success" %}
If you are not part of the group, feel free to use the optional `devMode` in your zkConnectConfig. Set the boolean field enabled as true and input a list of addresses that will be used instead of the actual group. Beware that this devMode should only be used when prototyping, if you keep such config in production, these dev addresses will be able to make proofs in their Vaults.
{% endhint %}



```typescript

import { AuthType, useZkConnect, ZkConnectClientConfig, ZkConnectButton, ZkConnectResponse } from "@sismo-core/zk-connect-react";
import { ethers } from "ethers";

const config: ZkConnectClientConfig = {
  appId: "0x11b1de449c6c4adb0b5775b3868b28b3", // zkDrop appId I created
  devMode: {
    enabled: true, // will use the Dev Sismo Data Vault https://dev.vault-beta.sismo.io/
    devGroups : [ // Will insert these addresses in data groups as eligible addresse
      {
        // the existing groupId you want to change the data from
        groupId: "0xe9ed316946d3d98dfcd829a53ec9822e",
        // the test data that will be used for the group chosen
        data: [
          "0x123...def",
          "0x456...abc"
        ],
      },
    ],
  }
}

<ZkConnectButton 
  ...
/>
```



After your user generates the proof, he will be automatically redirected back to your app.

You can use the `useZkConnect` react hook to get the response back as a typescript object and as bytes. The typescript object can be used to see the response in clear while the responseBytes is the [`zkConnectResponse`](../../technical-documentation/zkconnect/zkconnect-solidity-library-verify-on-chain-soon.md) encoded as bytes that you will send to your smart contracts.

```typescript
const { response, responseBytes } = useZkConnect({config: zkConnectConfig });
```



You can find the full frontend code snippet used for zkDrop [here](https://github.com/sismo-core/zkdrop/blob/main/front/src/pages/MergooorPass/index.tsx#L86).
{% endtab %}
{% endtabs %}

Well done! Now that the frontend is done, let's jump to the contracts! 💪

As you noticed, you now have the proof as bytes and you will send this proof in your contracts to check its validity with respect to `the-merge-contributor` group, the Data Vault ownership requested and the message signed by the user.&#x20;

### Verify the proof on-chain and gate your contracts

To be able to use the [zkConnect Solidity library](../../technical-documentation/zkconnect/zkconnect-solidity-library-verify-on-chain-soon.md), you will first need to install Foundry.&#x20;

```bash
curl -L https://foundry.paradigm.xyz | bash
```

{% hint style="info" %}
Here is the link to the Foundry book if you are not familiar with it: [https://book.getfoundry.sh/](https://book.getfoundry.sh/)
{% endhint %}

{% hint style="success" %}
We HIGHLY recommend to fork our`contracts` folder of zkDrop to be well setup : [https://github.com/sismo-core/zkdrop/tree/main/contracts](https://github.com/sismo-core/zkdrop/tree/main/contracts)
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

You can now put your contracts under `contracts/src` and start to use the [zkConnect Solidity Library](../../technical-documentation/zkconnect/zkconnect-solidity-library-verify-on-chain-soon.md).&#x20;

The first thing to setup is to make your contract inherit from zkConnect so that it can pass the appId in the constructor to configure zkConnect for your app (it is similar to the zkConnectConfig we have in our frontend).&#x20;

We also pass the groupId in the constructor to have it as an immutable in the contract.&#x20;

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

// we can use ZkConnect thanks to this import
import "./libs/zk-connect/ZkConnectLib.sol"; 
import {ERC721} from "@openzeppelin/contracts/token/ERC721/ERC721.sol";

// Your contract inherits from ZkConnect
contract ZKDropERC721 is ERC721, ZkConnect {
  // the group we want our users to be member of
  bytes16 public immutable GROUP_ID; 
  // the request content is an object that handles 
  // the claim, auth and message signature requests
  ZkConnectRequestContent private zkConnectRequestContent;
  
  ...
  
  constructor(
    string memory name,
    string memory symbol,
    string memory baseTokenURI,
    bytes16 appId,  // <- the appId registered in the Factory
    bytes16 groupId // <- the groupId of the-merge-contributor group
  ) ERC721(name, symbol) ZkConnect(appId) {
    GROUP_ID = groupId;
    _setBaseTokenURI(baseTokenURI);
  }

  ...
  
}
```

You can now start to implement new functions that take a `ZkConnectResponse` of type `bytes`.  It will allow you to send the `responseBytes` that you received in your frontend to your contracts. For example, here is the function that will be used for claiming the ERC721 by users.&#x20;

```solidity
// the function takes the zkConnectResponse as bytes 
// and the `to` address on which the user wants to claim the ERC721
function claimWithZkConnect(bytes memory zkConnectResponse, address to) public {
    // it verifies the response with respect to an Auth and a Claim
    // and returns a ZkConnectVerifiedResult
    ZkConnectVerifiedResult memory zkConnectVerifiedResult = verify({
        // the zkConnectResponse as bytes
        responseBytes: zkConnectResponse,
        // we recreate the Auth made earlier by the front 
        // to be sure that the proof is valid with respect to this Auth
        authRequest: buildAuth({authType: AuthType.ANON}),
        // we recreate the Claim made earlier by the front 
        // to be sure that the proof is valid with respect to this Claim
        claimRequest: buildClaim({groupId: GROUP_ID}),
        // we recreate the message signature request 
        // to be sure that the proof is made for this address
        messageSignatureRequest: abi.encode(to)
    });
    
    // we use the userId requested with the Auth request 
    // as the tokenId of the ERC721 to ensure that the same vault
    // can't claim to different ERC721 token
    uint256 tokenId = zkConnectVerifiedResult.verifiedAuths[0].userId;

    // we mint the ERC721 to the address
    _mint(to, tokenId);
  }
```

This function should give you a huge feeling on how to leverage the three type of requests available: the Claim request, the Auth request and the Signed Message request. By using them, you can prove a user group membership, an account ownership while making the user sign a message in his Data Vault.\
\
By calling the `claimWithZkConnect` function with the `zkConnectResponse` as bytes received by your front, you have now the full flow of an on-chain zkConnect integration. 💪

### Test your contracts

While following this tutorial, you obviously thought about testing your contracts before deploying them. To test them, you will need some custom proofs that can be generated easily with a script. \
The aim of the script is to mock your frontend client.

```typescript
import { ZkConnect } from "@sismo-core/zk-connect-client";
const opn = require("better-opn");
import {
  AuthType,
  ClaimType,
  RequestContentLib,
  ZkConnectClientConfig,
  ZkConnectRequestContent,
} from "@sismo-core/zk-connect-client";
import { ethers } from "ethers";

// This is the configuration needed to
// use the appId registered in the Sismo Factory
// and configure devMode if needed
const zkConnect = ZkConnect({
  appId: "0x11b1de449c6c4adb0b5775b3868b28b3",
  devMode: {
    // we want to see the proof as bytes to copy it in our tests
    displayRawResponse: true,
  },
} as ZkConnectClientConfig);

const abiCoder = new ethers.AbiCoder();

const url = zkConnect.getRequestLink({
  claimRequest: {
    groupId: "0xe9ed316946d3d98dfcd829a53ec9822e",
    groupTimestamp: "latest",
    value: 1,
    claimType: ClaimType.GTE,
    extraData: "",
  },
  authRequest: {
    authType: AuthType.ANON,
  },
  messageSignatureRequest: abiCoder.encode(
    ["address"],
    ["0x7def1d6D28D6bDa49E69fa89aD75d160BEcBa3AE"]
  ),
});

opn(url);
```

You can then call this script by using:

```bash
npx ts-node <script-name>.ts
```

You will be provided a ZkConnectResponse of type bytes in a modal by completing the flow in the Data Vault app. You can choose to copy paste this response in your tests.

<figure><img src="../../.gitbook/assets/Capture d’écran 2023-03-31 à 16.23.36.png" alt=""><figcaption><p>The zkConnectResponse as bytes</p></figcaption></figure>

You can test your contract by doing a fork test on the Goerli network for example. You will need a fork test because the zkConnect Solidity library interacts with a contract called `ZkConnectVerifier` already deployed on chain. You can copy paste the following test file in `contracts/test/ZkDropERC721.t.sol` and launch the following command:

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
import "zk-connect-solidity/SismoLib.sol";

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
        bytes memory zkConnectResponseEncoded =
            hex"000000000000000000000000000000000000000000000000000000000000002011b1de449c6c4adb0b5775b3868b28b300000000000000000000000000000000b8e2054f8a912367e38a22ce773328ff000000000000000000000000000000007a6b2d636f6e6e6563742d76320000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000800000000000000000000000000000000000000000000000000000000000000001000000000000000000000000000000000000000000000000000000000000002000000000000000000000000000000000000000000000000000000000000000c00000000000000000000000000000000000000000000000000000000000000180000000000000000000000000000000000000000000000000000000000000022068796472612d73322e310000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000002600000000000000000000000000000000000000000000000000000000000000540e9ed316946d3d98dfcd829a53ec9822e000000000000000000000000000000006c617465737400000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000001000000000000000000000000000000000000000000000000000000000000000100000000000000000000000000000000000000000000000000000000000000a000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000001000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000200000000000000000000000007def1d6d28d6bda49e69fa89ad75d160becba3ae00000000000000000000000000000000000000000000000000000000000002c00b64f961ab740d338d660cc4138c79225e8ecccb78a59dc52311169b6a00f70f27fb5a894b3c5344412bb00a63d13170c6dae480c4459fd41b3354f72064d11116b14847d2b64d7596286adaa13c3353bac911fc5fe0ac183b1775fc94b05218210b28d8fae8cc2bb2ea6d3a46cade59f01cafac0020ef38a21f3811279fb55f02e4a037e75c6a6b6b25ecf2979f65df7524cc0b2ab20d4270afb7610f1c49ae2372edacdf9eeb686cb4eee42490c1f918f4246fc4606b0d599e12bdacd20345249c36d3e6aeee76be3aaed8cd1f3d781ef91641f671cc9f44e3bda10643bd0d24b30bb0ed6d10e0663b8b1d6a4eeda5a868d54f63faa951f2ef159ce27556dd000000000000000000000000000000000000000000000000000000000000000009f60d972df499264335faccfc437669a97ea2b6c97a1a7ddf3f0105eda34b1d2ab71fb864979b71106135acfa84afc1d756cda74f8f258896f896b4864f025630423b4c502f1cd4179a425723bf1e15c843733af2ecdee9aef6a0451ef2db740a51e98a112191871021d36d20b4812488328eba74a8a23788893026af69af3604f81599b826fa9b715033e76e5b2fdda881352a9b61360022e30ee33ddccad90744e9b92802056c722ac4b31612e1b1de544d5b99481386b162a0b59862e0850000000000000000000000000000000000000000000000000000000000000001285bf79dc20d58e71b9712cb38c420b9cb91d3438c8e3dbaf07829b03ffffffc0000000000000000000000000000000000000000000000000000000000000000174c0f7d68550e40962c4ae6db9b04940288cb4aeede625dd8a9b0964939cdeb0000000000000000000000000000000011b1de449c6c4adb0b5775b3868b28b3000000000000000000000000000000000000000000000000000000000000000100000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000";
        zkdrop.claimWithZkConnect(zkConnectResponseEncoded, user);
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

Here it is! \
\
If you have any questions about integrating zkConnect, don’t hesitate to reach out. The team will be happy to answer any questions you may have.

Get involved in the Sismo community:

* Look out for [**hackathons**](https://www.notion.so/sismo/Sismo-x-ETHPorto-2023-cbda827ea5f2469aa5fdbb4955fc18d6?pvs=4) that we are participating in
* Join our [**Discord**](https://discord.gg/sismo) **** or our **** [**Dev Telegram**](https://t.me/+Z-SwcvXZFRVhZTQ0)****
* See the [**Sismo-hub contributing page**](https://github.com/sismo-core/sismo-hub/issues)****
