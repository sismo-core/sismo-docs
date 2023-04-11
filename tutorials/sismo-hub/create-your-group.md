---
description: Developer Tutorial
---

# Create your Group

## What’s inside?

This beginner-friendly tutorial will walk you through the creation of a [Group Generator](../../build-on-sismo/sismo-protocol-overview.md).

Group Generators are functions that enable the creation of [Groups](../../technical-documentation/zk-badge-protocol/groups.md) at the center of the Sismo protocol. Groups are the foundation of all what you can create with Sismo such as [sismoConnect Apps](../../what-is-sismo/sismoconnect.md) and [ZK Badges](../../technical-documentation/zk-badge-protocol/badges.md).

{% hint style="info" %}
You can find all already existing Group Generators [**here**](https://github.com/sismo-core/sismo-hub/tree/main/group-generators/generators)**.**
{% endhint %}

You can find the pull request associated with this tutorial [**here**](https://github.com/sismo-core/sismo-hub/pull/1484). It will give you a good feeling of what we are going to create in this tutorial! 👌

### What are [Groups](../../technical-documentation/zk-badge-protocol/groups.md)?&#x20;

[Groups](../../technical-documentation/zk-badge-protocol/groups.md) are composed of Data Sources:

* Web2 accounts: Twitter, Github
* Web3 accounts: Ethereum addresses, ENS, Lens handles

In order to create a Group, we need to build a [Group Generator](../../build-on-sismo/sismo-protocol-overview.md). This tutorial will guide you through the process of creating one.

There are 3 different ways to define the data of a Group generator, you can use:

* A hardcoded list of Data Source (e.g. [rhinofi-power-users](https://github.com/sismo-core/sismo-hub/blob/main/group-generators/generators/rhinofi-power-users/index.ts))
* [Data Providers](../../technical-documentation/sismo-hub/data-providers.md) (e.g. [proof-of-humanity](https://github.com/sismo-core/sismo-hub/blob/main/group-generators/generators/proof-of-humanity/index.ts))
* Already existing [Groups](../../technical-documentation/zk-badge-protocol/groups.md) (e.g. [sismo-contributors](https://github.com/sismo-core/sismo-hub/blob/main/group-generators/generators/sismo-contributors/index.ts))

If you want more info on Groups, check out this [page](../../technical-documentation/zk-badge-protocol/groups.md).

If you want to know how to contribute to the Sismo Hub by creating Groups don't hesitate to check the [Contributor Guide](https://github.com/sismo-core/sismo-hub/blob/main/CONTRIBUTING.md).

### Group use-cases

For now, you can create 2 different usages on top of Groups:

* [ZK Badges](../../technical-documentation/zk-badge-protocol/badges.md)
* [sismoConnect App](../../what-is-sismo/sismoconnect.md)

Let's take the [proof-of-humanity](https://github.com/sismo-core/sismo-hub/tree/main/group-generators/generators/proof-of-humanity) Group as an example and look at these use cases:

#### [ZK Badge](../../technical-documentation/zk-badge-protocol/badges.md)

Here you can see an [example](https://app.sismo.io/?badge=proof-of-humanity-zk-badge) of a Badge created from a Group:

<figure><img src="../../.gitbook/assets/Capture d’écran 2023-03-07 à 16.18.19.png" alt=""><figcaption><p>Proof of Humanity ZK Badge</p></figcaption></figure>

This Badge was generated from the [proof-of-humanity](https://github.com/sismo-core/sismo-hub/tree/main/group-generators/generators/proof-of-humanity) Group. The Sismo tech stack ensures that only Proof of Humanity registered addresses can mint the Badge (on the address of their choice) without revealing the registered address.&#x20;

#### [sismoConnect App](../../what-is-sismo/sismoconnect.md)

Here is an instance of a sismoConnect App that has been implemented using the proof-of-humanity Group:

<figure><img src="../../.gitbook/assets/Capture d’écran 2023-03-10 à 14.06.38 2 copie.png" alt=""><figcaption><p>sismoConnect App example</p></figcaption></figure>

This sismoConnect App allows you to gate contents/features of your app to Proof of Humanity registrants without revealing the registered addresses.&#x20;

After this tutorial, you will be able to create a [ZK Badge](../../technical-documentation/sismo-api/badge/) or a [sismoConnect App](../../what-is-sismo/sismoconnect.md) from your Group through the [Factory](https://factory.sismo.io/).

### Tutorial use-case

This tutorial will show you how to create a [Group](../../technical-documentation/zk-badge-protocol/groups.md) and make it usable for creating ZK Badge or sismoConnect App through the [Factory](https://factory.sismo.io/).

In the course of this tutorial, you will build your first Group named `tutorial-sismo-early-interactors`, containing all the collectors of the [first Sismo Lens post](https://lenster.xyz/posts/0x26e5-0x02) and all voters of one of the first [Sismo Proposals](https://snapshot.org/#/sismo.eth).

Here's a sample of the Group and the metadata you will get at the end of the tutorial:

<figure><img src="../../.gitbook/assets/Capture d’écran 2023-03-07 à 18.38.39 2.png" alt=""><figcaption><p>Group data sample</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/spaces_DbBfd4ahlvVGlRudj8NR_uploads_Jc2HZkel01QFenlapLLg_Capture d’écran 2023-03-07 à 18.webp" alt=""><figcaption><p>Group Metadata</p></figcaption></figure>

Let's build our Group now 🧑‍💻

### Setup of the local environment

First, clone the Sismo Hub repository and `cd` into it:

```bash
# using ssh
git clone git@github.com:sismo-core/sismo-hub.git
cd sismo-hub
```

You can now install all the dependencies with the `yarn` command:

```bash
# install dependencies
yarn
```

{% hint style="info" %}
You can install `yarn` easily [**here**](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-the-yarn-package-manager-for-node-js)**.**
{% endhint %}

### Creation of the Group Generator

Go to the `group-generators/generators` folder and create a new folder and a file inside.

```sh
# in a new terminal
# you are in sismo-hub

# cd into the folder
cd group-generators/generators
# create the group-generator folder for our new Group
mkdir tutorial-sismo-early-interactors
cd tutorial-sismo-early-interactors
# create the file
touch index.ts
```

You will build your Group in the `index.ts` file.

Before creating our tutorial use case, let's create a simple Group with a basic hardcoded list of accounts:

```typescript
// group-generators/generators/tutorial-first-sismo-post-collectors/index.ts

import {
  ValueType,
  Tags,
  GroupWithData,
} from "topics/group";
import {
  GenerationContext,
  GenerationFrequency,
  GroupGenerator,
} from "topics/group-generator";

const generator: GroupGenerator = {
  generationFrequency: GenerationFrequency.Once,

  generate: async (context: GenerationContext): Promise<GroupWithData[]> => {

    const jsonListData0 = {
      "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045": "1",
      "0xF61CabBa1e6FC166A66bcA0fcaa83762EdB6D4Bd": "1",
      "0x2E21f5d32841cf8C7da805185A041400bF15f21A": "1",
    };

    return [
      {
        // give a name to your Group
        name: "tutorial-sismo-early-interactors",
        timestamp: context.timestamp,
        // add a small description explaining how to be eligible to your Group
        description: "Be an early interactor in Sismo community",
        // document the Group eligibility criterias more specifically
        specs: "Collect the first Sismo Lens post (https://lenster.xyz/posts/0x26e5-0x02) or vote to the first new Sismo DAO proposal (https://snapshot.org/#/sismo.eth/proposal/0xe280e236c5afa533fc28472dd0ce14e5c3514a843c0563552c962226cda05c52)",
        // reference the final data we created
          // two different data formats in the Group
          // Lens account -> "dhadrien.lens": "1"
          // Ethereum account -> "0x95af97aBadA3b4ba443ff345437A5491eF332bC5": "1", 
        data: jsonListData0,
        valueType: ValueType.Info,
        tags: [Tags.User, Tags.Lens, Tags.Web3Social],
      },
    ];
  },
};

export default generator;
```

{% hint style="info" %}
Both name of the folder and of the group has to be in `kebab-case`

Example: `this-is-my-folder` or `this-is-my-group`
{% endhint %}

Once your Group Generator is built, you will then need to reference it in the `generators` folder:

```typescript
// group-generators/generators/index.ts

...
// you need to import your group generator
import tutorialSismoEarlyInteractors from "./tutorial-sismo-early-interactors"

import { GroupGeneratorsLibrary } from "topics/group-generator";

export const groupGenerators: GroupGeneratorsLibrary = {
  ...
  // you need to reference your group generator
  "tutorial-sismo-early-interactors": tutorialSismoEarlyInteractors,
};
```

You can now generate the first version of the `tutorial-sismo-early-interactors` Group!&#x20;

```bash
# command to generate the Group
yarn generate-group tutorial-sismo-early-interactors
```

> NB: You can add `--help` to know all the commands for `generate-group`

If the group generation went well, here is the log you should see on your terminal:

<figure><img src="../../.gitbook/assets/Capture d’écran 2023-03-08 à 14.25.10.png" alt=""><figcaption><p>Group generation succeeded</p></figcaption></figure>

As you can see we successfully generated the Group, and it contains the 3 elements (<mark style="color:red;">red box</mark>), this is the 3 accounts you defined in the `jsonListData0` variable.



Now that you have an overview of the process of creating a Group, let's move on to the tutorial use case! 🧑‍💻

As previously stated, we want to fetch all the collectors of the [first Sismo Lens post](https://lenster.xyz/posts/0x26e5-0x02) and all voters of one of the first [Sismo Proposals](https://snapshot.org/#/sismo.eth/proposal/0xe280e236c5afa533fc28472dd0ce14e5c3514a843c0563552c962226cda05c52).

To fetch all the accounts we want, you will use the 2 different [Data Providers](../../technical-documentation/sismo-hub/data-providers.md):&#x20;

* The [**Lens Data Provider**](https://github.com/sismo-core/sismo-hub/tree/main/group-generators/helpers/data-providers/lens): it will allow you to fetch all the collectors of the [first Sismo Lens Post](https://lenster.xyz/posts/0x26e5-0x02). (1.)

{% hint style="info" %}
If you want more info on how Data Provider works and how to create one, there is a tutorial for you [**here**](create-your-data-provider.md). This tutorial makes you create a Lens data provider and a function that fetch all Lens post collectors (It's a good time, isn't it 😉)
{% endhint %}

* The[ **Snapshot Data Provider**](https://github.com/sismo-core/sismo-hub/tree/main/group-generators/helpers/data-providers/snapshot): it will allow you to fetch all the voters of one of the first [Sismo proposals](https://snapshot.org/#/sismo.eth/proposal/0xe280e236c5afa533fc28472dd0ce14e5c3514a843c0563552c962226cda05c52) on Snapshot. (2.)

Once this data has been fetched, you'll need to merge it using [Data Operators](../../technical-documentation/sismo-hub/data-operators.md). A few different Data Operators that allow you to make operations on the data exist. Here you want to keep all the collectors and voters in the Group, so you will use the **Union** Data Operator. (3.)

```typescript
// group-generators/generators/tutorial-first-sismo-post-collectors/index.ts

import { dataOperators } from "@group-generators/helpers/data-operators";
import { dataProviders } from "@group-generators/helpers/data-providers";
import {
  ValueType,
  Tags,
  FetchedData,
  GroupWithData,
} from "topics/group";
import {
  GenerationContext,
  GenerationFrequency,
  GroupGenerator,
} from "topics/group-generator";

const generator: GroupGenerator = {
  generationFrequency: GenerationFrequency.Once,

  generate: async (context: GenerationContext): Promise<GroupWithData[]> => {
    // 1. Instantiate the Lens provider
    const lensProvider = new dataProviders.LensProvider();
    // query all the collectors of the first Sismo Lens post
    // https://lenster.xyz/posts/0x26e5-0x02
    const collectors: FetchedData = await lensProvider.getWhoCollectedPublication({
      publicationId: "0x26e5-0x02",
    });

    // 2. Instantiate the Snapshot provider
    const snapshotProvider = new dataProviders.SnapshotProvider();
    // query all the voters of the Sismo space on snapshot
    // https://snapshot.org/#/sismo.eth/proposal/0xe280e236c5afa533fc28472dd0ce14e5c3514a843c0563552c962226cda05c52
    const voters: FetchedData = await snapshotProvider.queryProposalVoters({
      proposal: "0xe280e236c5afa533fc28472dd0ce14e5c3514a843c0563552c962226cda05c52"
    });

    // 3. Make a union of the two queried data
    const tutorialSismoActiveMembers = dataOperators.Union([
      collectors,
      voters,
    ]);

    return [
      {
        // give a name to your Group
        name: "tutorial-sismo-early-interactors",
        timestamp: context.timestamp,
        // add a small description explaining how to be eligible to your Group
        description: "Be an early interactor in Sismo community",
        // document the Group eligibility criterias more specifically
        specs: "Collect the first Sismo Lens post (https://lenster.xyz/posts/0x26e5-0x02) or vote to the first new Sismo DAO proposal (https://snapshot.org/#/sismo.eth/proposal/0xe280e236c5afa533fc28472dd0ce14e5c3514a843c0563552c962226cda05c52)",
        // reference the final data we created
          // two different data formats in the Group
          // Lens account -> "dhadrien.lens": "1"
          // Ethereum account -> "0x95af97aBadA3b4ba443ff345437A5491eF332bC5": "1", 
        data: tutorialSismoActiveMembers,
        valueType: ValueType.Info,
        tags: [Tags.User, Tags.Lens, Tags.Web3Social],
      },
    ];
  },
};

export default generator;
```

And... 🥁 It's already finished!



As you may notice, the `data` field is where the Group of Data Sources is stored, and its type is a `FetchedData`:

```typescript
export type FetchedData = {
    [address: string]: BigNumberish;
};
```

And here's an example of what a `FetchedData` object looks like:

```json
{
  "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045": "1",
  "0xF61CabBa1e6FC166A66bcA0fcaa83762EdB6D4Bd": "1",
  "0x2E21f5d32841cf8C7da805185A041400bF15f21A": "1",
}
```

Here there is only "`0x123...`" type addresses but the `fetchedData` object can also contain other different types of accounts:

* ENS (e.g. `"sismo.eth":"1"`)
* Lens handles (e.g. `"sismo.lens":"1"`)
* GitHub accounts (e.g. `"github:leosayous21":"1"`)
* Twitter accounts (e.g. `"twitter:leosayous21":"1"`)

{% hint style="info" %}
During the Group generation, all accounts are resolved into an Ethereum address through a [resolver](https://github.com/sismo-core/sismo-hub/tree/main/src/topics/resolver). There is a resolver for each type of Data Source.
{% endhint %}

Last but not least, you may have noticed the `"1"` at the end of each account in a Group. This is called: the value, and it allows you to add granularity to your Group. For our tutorial, we don't want to make distinction between each account in the Group, but if we wanted to, we could set a value of 2 for all collectors for instance.

{% hint style="info" %}
Here is an example of how to use this value: at Sismo, we use a Sismo Contributor ZK Badge for voting on Sismo DAO Proposals. There is 3 different value in the Group:

* "1" --> get 1 voting power
* "2" --> get 50 voting power
* "3" --> get 500 voting power
{% endhint %}

For this tutorial, you only use Lens and Snapshot Data Providers which don't require API key. But for other Data Providers (e.g. [BigQuery](https://github.com/sismo-core/sismo-hub/tree/main/group-generators/helpers/data-providers/big-query), [Hive](https://github.com/sismo-core/sismo-hub/tree/main/group-generators/helpers/data-providers/hive)...) or resolvers (e.g. [Twitter](https://github.com/sismo-core/sismo-hub/blob/main/src/topics/resolver/twitter-resolver.ts)) an API key is required. So if you want to set an API key for the Data Provider you want to use, you need to create an environment variable. And here is how to do it:

<details>

<summary>Data Provider setup</summary>

```sh
# in a terminal
# you are in sismo-hub root

cp .example.env .env
```

Then you can add your own API key by adding a new line to this file:

```properties
# in the .env file
export <THE_DATA_PROVIDER_YOU_WANT_TO_USE>="<YOUR_API_KEY>"
```

And you need to export the variable in order for you to use it locally on your computer:

```sh
# in a terminal
# you are in sismo-hub root

source .env
```

For instance, If you want to add Twitter accounts to your group, you will need a Twitter API Key. Here's how to get one : [https://developer.twitter.com/en/docs/twitter-api/getting-started/getting-access-to-the-twitter-api](https://developer.twitter.com/en/docs/twitter-api/getting-started/getting-access-to-the-twitter-api)

</details>

You can now, once again, generate the `tutorial-sismo-early-interactors` Group:

```bash
# command to generate the Group
yarn generate-group tutorial-sismo-early-interactors
```

If the Group generation went well, here is the log you should see on your terminal:

<figure><img src="../../.gitbook/assets/spaces_DbBfd4ahlvVGlRudj8NR_uploads_Ar7MPrJaccllR1TdBBuh_Capture d’écran 2023-03-08 à 14.webp" alt=""><figcaption><p>Group generation succeeded</p></figcaption></figure>

The Group has been successfully generated and it contains 1550 accounts (<mark style="color:red;">red box</mark>) at the time of the tutorial. But you may have a different number depending on the number of new collectors from the Sismo Lens post.

As you can see on the screenshot, after the generation, the command returns us some information about the Group you just generated.

You can retrieve the Group data at the path indicated in <mark style="color:yellow;">yellow</mark>, and if you want to see a sample of the Group you just generated:

<pre class="language-sh"><code class="lang-sh"># in a terminal
# this will show you the first 10 line of the Group
head -10 &#x3C;<a data-footnote-ref href="#user-content-fn-1">yellow-link</a>>
</code></pre>

Then the 2 others blue paths are API endpoints that allow you to get from the Group its data and metadata. To use the API:

<details>

<summary>Sismo Hub API setup</summary>

```sh
# in a new terminal
# at the Sismo Hub repository root
yarn api:watch
```

Type on your web browser: [http://127.0.0.1:8000](http://127.0.0.1:8000/static/rapidoc/index.html)<<mark style="color:blue;">blue-link</mark>>

NB: You can go to [http://127.0.0.1:8000/static/rapidoc/index.html](http://127.0.0.1:8000/static/rapidoc/index.html) to see the main endpoints of the Sismo Hub API

Finally, you should see what is displayed on the screenshot in [#tutorial-use-case](create-your-group.md#tutorial-use-case "mention")

</details>

### See your work integrated on Sismo

Your Group Generator is now ready, so it's time to push it in prod!

You don't have to store anything or launch anything on your side. You will  just have to add [a pull request to the Sismo Hub](https://github.com/sismo-core/sismo-hub/pulls) to see your Group Generator live! 😁

First, you will have to [fork](https://docs.github.com/en/get-started/quickstart/fork-a-repo) the [Sismo Hub repository](https://github.com/sismo-core/sismo-hub):

<figure><img src="../../.gitbook/assets/spaces_DbBfd4ahlvVGlRudj8NR_uploads_i1jUXC9Z3fjE8ww5tIr4_Capture d’écran 2023-02-28 à 20.webp" alt=""><figcaption><p>Fork Sismo Hub repository</p></figcaption></figure>

Then you will have to add a new remote to your repo:

```shell
git remote add fork <your_fork_repo_link.git>
```

You can also create a new branch for your work:

```sh
git checkout -b tutorial-sismo-early-interactor-group
```

In this tutorial you should have 2 different files changed:

* 1 file created: `group-generators/generators/tutorial-sismo-early-interactors/index.ts`
* 1 file changed: `group-generators/generators/index.ts`

Next, add all your changes and push them to your fork:

```bash
# git add the two files
git add .
git commit -m "feat: add tutorial-sismo-early-interactors group generator"
# if you created the branch tutorial-sismo-early-interactor-group
git push fork tutorial-sismo-early-interactor-group
# otherwise
git push
```

You will then see on your forked repository the following message, you can click on "Compare & pull request" to begin creating your pull request:

<figure><img src="../../.gitbook/assets/Capture d’écran 2023-03-07 à 19.04.21 2.png" alt=""><figcaption><p>Click on "Compare &#x26; pull request" to begin creating your PR</p></figcaption></figure>

You will finally see your branch being compared to the main branch of the [Sismo Hub](https://github.com/sismo-core/sismo-hub/) repository (<mark style="color:blue;">blue boxes</mark>). You can review your changes and add a meaningful title and comments, and when you are happy with your PR, click "Create Pull request". 😇

<figure><img src="../../.gitbook/assets/Capture d’écran 2023-03-07 à 19.17.05.png" alt=""><figcaption><p>Create your pull request</p></figcaption></figure>

For example, here is a valid pull request: [https://github.com/sismo-core/sismo-hub/pull/1484](https://github.com/sismo-core/sismo-hub/pull/1484)

When your PR is merged your Group generator will be registered in the Sismo Hub, and your Group will be generated and sent onchain (through the [Registry Tree](../../technical-concepts/accounts-registry-tree.md)).

Finally, your Group will be available in the Sismo Hub and anyone will be able to use it to create **Badges** or **sismoConnect**, great job! 💪

### Use your Group

Now that you have your Group, you can build 2 different things from it:

* [**ZK Badge**](../../technical-documentation/sismo-api/badge/):
  * using the [Factory](https://factory.sismo.io/badges-explorer)
  * creating by yourself through the Sismo Hub. And **here**(soon™) is a tutorial that shows how to build one.
* [**sismoConnect App**](../../what-is-sismo/sismoconnect.md):
  * using the [Factory](https://factory.sismo.io/apps-explorer)
  * creating by yourself using the [sismo-connect-packages](https://github.com/sismo-core/sismo-connect-packages). And **here**(soon™) is a tutorial that shows how to build one.

### Contribute to the Sismo Hub

You want to contribute but you don't have any ideas for Groups to create? Check out the current [Sismo Hub GitHub's issues](https://github.com/sismo-core/sismo-hub/issues) for some interesting ideas.

If you have any questions or you need help regarding your Group creation process, do not hesitate to join [Sismo Discord](https://discord.gg/sismo) and ask us in **#🙋︱dev-support**. We will be glad to answer you 🤗

[^1]: 
