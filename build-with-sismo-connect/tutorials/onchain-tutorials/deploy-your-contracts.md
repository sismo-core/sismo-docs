# Deploy your contracts

## Overview

This tutorial aims at describing the notions around the deployment of Sismo Connect contracts. In this context, we will recap what is a Sismo Connect Client config and how to tune its `devMode` and `devGroups` properties. Once understood, these notions will allow you to confidently deploy your contracts inheriting Sismo Connect functionalities.

{% hint style="success" %}
To better understand the different notions highlighted in this tutorial, we strongly advise you to go through [**the first tutorial**](tuto.md) that will show you how to leverage data aggregation in your app by [**building a Gated Airdrop for Gitcoin Passport holders**](tuto.md).&#x20;
{% endhint %}

For this tutorial, we will again use the GitHub repository used for the [**gated airdrop tutorial**](tuto.md). You can **clone it** and **install it** as described in the repository's README or in [**the tutorial**](tuto.md). You don't need to clone it again if you just finished the tutorial.&#x20;

## Prerequisites

This tutorial requires:

* [Node.js](https://nodejs.org/en/download/) >= 18.15.0 (Latest LTS version)
* [Yarn](https://classic.yarnpkg.com/en/docs/install)
* [Foundry](https://book.getfoundry.sh/) (see how to install it [here](https://book.getfoundry.sh/getting-started/installation))&#x20;
* Metamask installed in your browser

{% hint style="success" %}
We use Foundry for our smart contract dependencies but we also have a [**Hardhat library**](https://www.npmjs.com/package/@sismo-core/sismo-connect-solidity). This tutorial is focused on how to deploy your smart contracts using Foundry.
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

Once this steps are done, you are well setup to use [Forge](https://book.getfoundry.sh/forge/deploying), a tool provided by Foundry to test and deploy smart contracts.&#x20;

## Understand the Sismo Connect client config

Before deploying any Sismo Connect contract, it is better to understand the Sismo Connect client config and especially the `devMode` and `devGroups` fields that you will often use in a local environment.&#x20;

As you may already know, Sismo Connect is the communication layer allowing any Sismo Connect app to requests some proofs about user data and receive the expected proofs before verifying them. To be able to produce such proofs, users are required to import accounts ([data sources](../../../knowledge-base/resources/understanding-sismo-glossary.md)) into their [Sismo Vaults](../../../knowledge-base/resources/technical-concepts/data-gems-and-data-groups.md). By doing so, they will be able to prove some group memberships and account ownerships to apps.&#x20;

`devMode` and `devGroups` are parameters of the Sismo Connect Client configuration that allows to choose between **developer vaults** and **production vaults**. The two vaults are separated in our infrastructure and the data imported in each vaults are different. It is the field `enabled` in `devMode` that determines which vault to redirect your users to in order to generate some proofs.

It is also important to note that developer vaults are working with contracts on testnet chains (Goerli, Mumbai) and production vaults are working on mainnet chains (Ethereum, Polygon and Gnosis).

`devGroups` is another property of `devMode` that allows to override existing groups with any addresses. It is pretty useful as a developer to simulate group membership eligibility but the proofs created thanks to such "fake" groups will only be verified if you change the groups onchain. Therefore, it requires to have a local chain to change such a thing.&#x20;

You can learn more about the Sismo Connect Client configuration in the Sismo Connect Client [**technical documentation**](../../technical-documentation/client.md).

```typescript
export const sismoConnectConfig: SismoConnectClientConfig = {
  appId: "0xf4977993e52606cfd67b7a1cde717069",
  devMode: {
    // users will be redirect to their developer vault
    // at https://dev.vault-beta.sismo.io/
    enabled: true,
    devGroups: [
      {
        // Gitcoin Passport group
        // https://factory.sismo.io/groups-explorer?search=0x1cde61966decb8600dfd0749bd371f12
        // this groupId should exist 
        // it will be the group that will be overriden with the chosen data
        groupId: "0x1cde61966decb8600dfd0749bd371f12",
        // this array will override all the data of the existing group chosen
        // it will simulate group membership eligibility for the addresses defined below
        // while it allows developers to generate proofs of group membership
        // it does not mean that proofs are going to be always verified
        // you will also need to have this configuration in your backend for example
        // OR setup a local chain if you are working with smart contracts
        // 
        // IMPORTANT NOTE: the proofs generated from these addresses
        // can't be verified on testnet and mainnet chains 
        // if you are verifying with smart contracts, you must have a local chain
        // and manually register the new root for your groups manually
        data: [
          // add your dev addresses in the array
          bobDevAddress,
          aliceDevAddress,
          // other addresses to test
          "0x2b9b9846d7298e0272c61669a54f0e602aba6290",
          "0xb01ee322c4f028b8a6bfcd2a5d48107dc5bc99ec",
          "0x938f169352008d35e065F153be53b3D3C07Bcd90",
        ],
      },
    ]
  },
};
```

{% hint style="success" %}
As a smart contract developer, you can keep the following model in mind when developing.\
You can simply think that three different environments are provided to you:

* the **mainnet** environment, works on mainnets with production groups and production vaults. (devMode is not enabled)
* the **testnet** environement, works on testnets with production groups and developer vaults. (devMode is enabled but without devGroups)
* the local environment, works on a local chain with fake groups and developer vaults. (devMode is enabled and devGroups are used).
{% endhint %}

## The deployment script

Solidity scripting is a way to declaratively deploy contracts using Solidity, instead of using the more limiting and less user friendly [`forge create`](https://book.getfoundry.sh/reference/forge/forge-create.html). You can look at the [example displayed in the Foundry documentation](https://book.getfoundry.sh/tutorials/solidity-scripting?highlight=script#solidity-scripting) if you want to learn more about it.&#x20;

We are going to use a simple solidity script to deploy our contract on our chosen network.&#x20;

You can see the script used in `script/Airdrop.s.sol` file. Here is it:

```solidity
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.13;

import "forge-std/Script.sol";
import "forge-std/console.sol";
import {Airdrop} from "src/Airdrop.sol";

contract DeployAirdrop is Script {
    function run() public {
        vm.startBroadcast();
        Airdrop airdrop = new Airdrop("My airdrop contract", "AIR");
        console.log("Airdrop Contract deployed at", address(airdrop));
        vm.stopBroadcast();
    }
}
```

This script is really simple, it declares a **new Airdrop contract** between `vm.startBroadcast` and `vm.stopBroadcast,` two [**foundry cheatcodes**](https://book.getfoundry.sh/cheatcodes/) that will simulate the contract creation in the context you will provide (here the context of a Mumbai fork as you will see).

You can simply run this command to see the simulation of your contract deployment in the context of a Mumbai fork.

```bash
forge script DeployAirdrop --rpc-url <your-mumbai-rpc-url>

# if you dont have any personal rpc url
# you can try to use https://rpc.ankr.com/polygon_mumbai
```

```bash
# output
== Logs ==
  Airdrop Contract deployed at 0x34A1D3fff3958843C43aD80F30b94c510645C316

EIP-3855 is not supported in one or more of the RPCs used.
Unsupported Chain IDs: 80001.
Contracts deployed with a Solidity version equal or higher than 0.8.20 might not work properly.
For more information, please see https://eips.ethereum.org/EIPS/eip-3855

## Setting up (1) EVMs.

==========================

Chain 80001

Estimated gas price: 3.000000038 gwei

Estimated total gas used for script: 5442749

Estimated amount required: 0.016328247206824462 ETH

==========================

SIMULATION COMPLETE. To broadcast these transactions, add --broadcast and wallet configuration(s) to the previous command. See forge script --help for more.
```

As you can see in the output logs, the simulation of the airdrop contract deployment was successful. You can even see the amount that will be required for you to deploy on the Mumbai Testnet. (You can use this Alchemy faucet if you need some testnet tokens: [https://mumbaifaucet.com/](https://mumbaifaucet.com/))

Since we are happy with the the script simulation, we can broadcast it on Mumbai by inputing an EOA private key in the command line.

{% hint style="success" %}
We strongly advise you to use a private key from a developer account when developing, do not leak any private key linked to some accounts with real assets in it.
{% endhint %}

Here is how to deploy the airdrop smart contract on the Mumbai testnet network. We use a publicly known private key to show you how to do it.

{% code overflow="wrap" %}
```bash
forge script DeployAirdrop --rpc-url <your-mumbai-rpc-url> --private-key '0xac0974bec39a17e36ba4a6b4d238ff944bacb478cbed5efcae784d7bf4f2ff80' --broadcast

# if you dont have any personal rpc url
# you can try to use https://rpc.ankr.com/polygon_mumbai
```
{% endcode %}

If you try to run this exact command without changing the private key, you will encounter an error since the account has no funds. You should replace the private key by your own developer private key and have some funds on your dev account (Mumbai faucet link: [https://mumbaifaucet.com/](https://mumbaifaucet.com/)).

{% hint style="success" %}
Notice the `--broadcast` option which states that you want to actually trigger the transaction on the Mumbai testnet.
{% endhint %}

## Verify your contracts with `--verify` option

To verify your contracts, you will need to register for an Etherscan API key.&#x20;

{% hint style="success" %}
The Etherscan API Key needs to be registered from Polygonscan if you want to deploy on Mumbai: [https://polygonscan.com/myaccount](https://polygonscan.com/myaccount)
{% endhint %}

You just need to directly specify the `--verify` option when deploying alongside your Etherscan api key.

It is pretty straight forward:

{% code overflow="wrap" %}
```bash
forge script DeployAirdrop \
--rpc-url <your-mumbai-rpc-url> \
--private-key '0xac0974bec39a17e36ba4a6b4d238ff944bacb478cbed5efcae784d7bf4f2ff80' \
--broadcast \
--etherscan-api-key <your_etherscan_api_key> \
--verify \
--watch

# if you dont have any personal rpc url
# you can try to use https://rpc.ankr.com/polygon_mumbai
```
{% endcode %}

```bash
# output
==========================

ONCHAIN EXECUTION COMPLETE & SUCCESSFUL.
Total Paid: 0.00837346 ETH (4186730 gas * avg 2 gwei)
##
Start verification for (1) contracts
Start verifying contract `0x1465ebb35e84a307a59e9c38d9ec3e773683b706` deployed on mumbai

Submitting verification for [src/Airdrop.sol:Airdrop] "0x1465EbB35e84a307A59E9c38D9Ec3E773683b706".

Submitting verification for [src/Airdrop.sol:Airdrop] "0x1465EbB35e84a307A59E9c38D9Ec3E773683b706".

Submitting verification for [src/Airdrop.sol:Airdrop] "0x1465EbB35e84a307A59E9c38D9Ec3E773683b706".
Submitted contract for verification:
	Response: `OK`
	GUID: `cgfkzkpdki9c2jlnwpy9lzrhpuckxhadf4yyqysm38jzmqscgi`
	URL:
        https://mumbai.polygonscan.com/address/0x1465ebb35e84a307a59e9c38d9ec3e773683b706
Contract verification status:
Response: `NOTOK`
Details: `Pending in queue`
Contract verification status:
Response: `OK`
Details: `Pass - Verified`
Contract successfully verified
All (1) contracts were verified!
```

{% hint style="success" %}
If you deploy the exact contract from the tutorial on Mumbai, you are likely to encounter a message stating "Already verified". As long as your contract is verified, it is totally fine!
{% endhint %}

And congrats for your deployment! You should be left with a verified contract at this point, if not don't hesitate to reach out to us on our [**developer telegram**](https://t.me/+Z-SwcvXZFRVhZTQ0)**.**

If you want to use the tutorial frontend with your contracts, you will need to add some minor changes in the `front/src/pages/claim-airdrop.tsx` file. You will first need to change how you get the `contractAddress` by changing 5151111 (the fork chain id) to 80001 (the Mumbai chain id) in your imports. And you also have to use the `polygonMumbai` chain config from viem isntead of `mumbaiFork` config for the chain.

```typescript
// you are in: front/src/pages/claim-airdrop.tsx

// import { transactions } from "../../../broadcast/Airdrop.s.sol/5151111/run-latest.json";
import { transactions } from "../../../broadcast/Airdrop.s.sol/80001/run-latest.json";
// ------------------------------------------------------------^^^^^ 
//                                                        REPLACE HERE
// add also the viem import for polygonMumbai chain
import { polygonMumbai } from "viem/chains";

// replace
// const userChain = mumbaiFork;
const userChain = polygonMumbai;
```

## Next steps

If you have any questions about integrating sismoConnect, don’t hesitate to reach out. The team will be happy to answer any questions you may have. Any feedback is also welcomed!

Get involved in the Sismo community! 🎭

* Look out for [**hackathons**](https://www.notion.so/sismo/Sismo-x-ETHPorto-2023-cbda827ea5f2469aa5fdbb4955fc18d6?pvs=4) that we are participating in
* Join our [**Discord**](https://discord.gg/sismo) or our [**Dev** **Telegram**](https://t.me/+Z-SwcvXZFRVhZTQ0)
* See the [**Sismo-hub contributing page**](https://github.com/sismo-core/sismo-hub/issues)
