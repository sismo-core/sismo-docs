# Onchain Sample Project

## Prerequisites

* [Node.js](https://nodejs.org/en/download/) >= 18.15.0 (Latest LTS version)
* [Yarn](https://classic.yarnpkg.com/en/docs/install)
* [Foundry](https://book.getfoundry.sh/) (see how to install it [here](https://book.getfoundry.sh/getting-started/installation))
* Metamask installed in your browser

## Installation

In a first terminal:

```bash
# clone the repository
git clone https://github.com/sismo-core/sismo-connect-onchain-sample-project.git
cd sismo-connect-onchain-sample-project

# install frontend dependencies
yarn

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
# this will start your Next.js app
# the frontend is available on http://localhost:3000/
# it deploys the contracts on the local blockchain
yarn dev
```

After this command, you will have your local application running on [http://localhost:3000](http://localhost:3000) and all the contracts have been deployed on your local blockchain.&#x20;

<figure><img src="../../.gitbook/assets/Capture d’écran 2023-05-12 à 11.02.45.png" alt="" width="375"><figcaption><p>Running app on <a href="http://localhost:3000">http://localhost:3000</a></p></figcaption></figure>

If you wish to see the frontend code, you can go to the `src/pages` folder. The contract code is in the `contracts` folder.

#### Be eligible for the airdrop

To be eligible for the airdrop, you will then need to add your address in the `config.ts` file:

```typescript
// config.ts

// Replace with your address to become eligible for the airdrop
// make sure to have this address in your Vault
export const yourAddress = "0x855193BCbdbD346B423FF830b507CBf90ecCc90B";
```

After changing the `config.ts` file, you don't need to restart the app or restart anvil. Simply refresh the page on the front end and you are good to go.

{% hint style="success" %}
&#x20;Make sure to import the address you reference in the `config.ts` file into your Data Vault when you try out the sample project application. If it is not done when you are redirected, you can add your address by clicking on the purple "connect" button.
{% endhint %}

<figure><img src="../../.gitbook/assets/Screenshot 2023-05-11 at 16.57.13.png" alt=""><figcaption><p>Import the address referenced in the config.ts file</p></figcaption></figure>

## Important note

{% hint style="warning" %}
The interaction with the fork network can become quite unstable if you stop the `yarn anvil` command at some point or if you have already used the sample app before.

If so:

* keep the local anvil node running,&#x20;
* make sure to delete your activity tab for the fork network in Metamask by going to "Settings > Advanced > Clear activity tab data" when connected to the fork network.&#x20;
* relaunch the anvil node and the application

See [FAQ](../faq.md) for more information.
{% endhint %}
