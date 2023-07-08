# Onchain Boilerplate

This boilerplate can be used as a base to start coding onchain applications that use Sismo Connect. It is using Next.js for frontend and Foundry for smart contracts development.

You are invited to modify:

* `front/src/app/page.tsx` , the code of the frontend using Sismo Connect React Button to request ZK Proofs
* `src/Airdrop.Sol` , the code of the smart contract using Sismo Connect Solidity Library to verify ZK Proofs\


{% hint style="success" %}
Use the [Sismo Connect Cheatsheet](../sismo-connect-cheatsheet.md) to chose your own data requests
{% endhint %}

This boilerplates implements SafeDrop, a Sybil-resistant airdrop from privately-aggregated data explained.

{% hint style="info" %}
Read the [full case study](https://case-studies.sismo.io) of SafeDrop or complete its [tutorial](../tutorials/tuto.md)
{% endhint %}

## Prerequisites

* [Node.js](https://nodejs.org/en/download/) >= 18.15.0 (Latest LTS version)
* [Yarn](https://classic.yarnpkg.com/en/docs/install)
* [Foundry](https://book.getfoundry.sh/) (see how to install it [here](https://book.getfoundry.sh/getting-started/installation))
* Metamask installed in your browser

## Installation

Repository: [https://github.com/sismo-core/sismo-connect-boilerplate-onchain](https://github.com/sismo-core/sismo-connect-boilerplate-onchain)

In a first terminal:

```bash
# clone the repository
git clone https://github.com/sismo-core/sismo-connect-boilerplate-onchain
cd sismo-connect-boilerplate-onchain

# install contract dependencies with Forge
foundryup
forge install
```

## Launch a local fork chain of Mumbai

In a new terminal:

```bash
yarn anvil
```

## Start your local application

In a new terminal:

```bash
# install frontend dependencies
cd front
yarn

# this will start your Next.js app
# the frontend is available on http://localhost:3000/
# it deploys the contracts on the local blockchain
yarn dev
```

After this command, you will have your local application running on [http://localhost:3000](http://localhost:3000) and all the contracts have been deployed on your local blockchain.&#x20;

<figure><img src="../../.gitbook/assets/Capture d’écran 2023-06-22 à 22.28.29.png" alt=""><figcaption><p>Running app on <a href="http://localhost:3000">http://localhost:3000</a></p></figcaption></figure>

If you wish to see the frontend code, you can go to the `front/src/app/page.tsx` file. The contract code is in the `src` folder.

{% hint style="success" %}
By default, the boilerplate lets you impersonate accounts to be eligible, if you want to learn more about it you can read about the [Sismo Connect Configuration](../technical-documentation/sismo-connect-configuration.md).
{% endhint %}

## Important note

{% hint style="warning" %}
The interaction with the fork network can become quite unstable if you stop the `yarn anvil` command at some point or if you have already used the sample app before.

If so:

* keep the local anvil node running,&#x20;
* make sure to delete your activity tab for the fork network in Metamask by going to "Settings > Advanced > Clear activity tab data" when connected to the fork network.&#x20;
* relaunch the anvil node and the application

See [FAQ](../faq.md) for more information.
{% endhint %}
