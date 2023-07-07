---
description: Developer tutorial
---

# Sismo Hub Guide: Add a Data Provider to the Sismo Factory

## What's inside?

This tutorial will explain how to create a [Data Provider](../../how-sismo-works/resources/sismo-hub/data-providers.md) in the [Sismo Hub](https://github.com/sismo-core/sismo-hub). Users (through the [Factory](https://factory.sismo.io)) and developers (through the [Sismo Hub](../../how-sismo-works/resources/sismo-hub/)) will be able to use your Data Provider in order to create [Group Generators](../../how-sismo-works/resources/sismo-hub/sismo-protocol-overview.md).

Group Generators are functions that enable the creation of groups at the center of the Sismo protocol. As groups need to contain data, we need Data Providers to fetch data from external APIs (e.g, Lens, GitHub, Snapshot).

{% hint style="info" %}
You can find all already existing Data Providers [**here**](https://github.com/sismo-core/sismo-hub/tree/main/group-generators/helpers/data-providers)**.**
{% endhint %}

By following this tutorial you will learn how to create a new Data Provider to fetch the data for your desired group.

Data providers are at the foundation of the Sismo Hub architecture. They are designed to abstract the complexity of fetching external APIs for future devs or group creators gathering data. They are meant to be easy to use and reliable for future group generation.

{% hint style="info" %}
You can find the pull request associated with this tutorial [**here**](https://github.com/sismo-core/sismo-hub/pull/1407/files)**.**
{% endhint %}

## What are [Data Providers](../../how-sismo-works/resources/sismo-hub/data-providers.md)?

Data Providers enable other group creators to reuse specific data fetching logic, such as calls to APIs.

[Groups](../../knowledge-base/resources/sismo-hub/sismo-hub/broken-reference/) are composed of Data Source:

* Web2 accounts: Twitter, GitHub, Telegram
* Web3 accounts: Ethereum addresses, ENS, Lens handle

When creating a group, you need to create a [Group Generator](../../how-sismo-works/resources/sismo-hub/sismo-protocol-overview.md). There are 3 different ways to do so:

* Enter a hardcoded list of accounts
* Use an already existing [Group Generator](../../how-sismo-works/resources/sismo-hub/sismo-protocol-overview.md)
* Use a [Data Provider](../../how-sismo-works/resources/sismo-hub/data-providers.md) to fetch accounts

Moreover, Data Providers are useful because they enable automatic and frequent group updates, whereas if the group was made of a hardcoded list of accounts it wouldn't be automatic.

{% hint style="success" %}
If you want to create a [group](../../knowledge-base/resources/sismo-hub/sismo-hub/broken-reference/) that contains all the voters of your last DAO proposal on Snapshot, simply use the `queryProposalVoters` function from the [Snapshot Data Provider](https://github.com/sismo-core/sismo-hub/tree/main/group-generators/helpers/data-providers/snapshot) in your [Group Generator](../../how-sismo-works/resources/sismo-hub/sismo-protocol-overview.md) by giving the Proposal Identifier as an argument.

For example:

```typescript
const snapshotProviderData0 = await snapshotProvider.queryProposalVoters({
      proposal: "0x864685d094312bc51eeb01af69e83dc7c496108cca54a15b491d79e9ce389889"
    });
```

This will fetch all the voters of the CoW Swap proposal number 23 (CIP-23)
{% endhint %}

If you want to know more on Data Providers, check out this [page](../../how-sismo-works/resources/sismo-hub/data-providers.md). Alternatively, you can check out the [Contributor Guide](https://github.com/sismo-core/sismo-hub/blob/main/CONTRIBUTING.md) before creating one yourself.

## Tutorial use case

This tutorial will show you how to create a Data Provider and make it usable in the Factory.

We will take the example of a **Lens Data Provider** on top of the Lens API and build our first query that gets the list of collectors of a specific Lens post.

At the end of this tutorial, it will be up to you to do a pull request on the [Sismo Hub](https://github.com/sismo-core/sismo-hub) to see your Data Provider integrated and be usable in the [Factory](https://factory.sismo.io/) (<mark style="color:green;">green box</mark>) and in the Sismo Hub (<mark style="color:red;">red box</mark>) 🙌

<figure><img src="../../.gitbook/assets/Capture d’écran 2023-03-03 à 13.45.55 2 (1).png" alt=""><figcaption><p>Use of the data provider</p></figcaption></figure>

## Tutorial

### Setup of the local environment

First, you need to clone the Sismo Hub repository

```sh
# using ssh
git clone git@github.com:sismo-core/sismo-hub.git
cd sismo-hub

# install dependencies
yarn
```

{% hint style="info" %}
You can install `yarn` [**here**](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-the-yarn-package-manager-for-node-js)**.**
{% endhint %}

### Creation of the Data Provider

Let's build our Data Provider now 🧑‍💻

Go to the `group-generators/helpers/data-providers` folder and create a new folder:

```sh
# in a new terminal
# you are in sismo-hub

cd ./group-generators/helpers/data-providers
# create the data-provider folder
mkdir tutorial-lens
cd tutorial-lens
```

For our case in this tutorial, we will split our Data Provider into 3 different files:

* `index.ts`: <mark style="color:orange;">\[mandatory]</mark> Contains all the functions you make available to use in a group generator. It will help format data that come from your API queries.
* `types.ts`: <mark style="color:orange;">\[mandatory]</mark> The file where you can define all the types you need.
* `queries.ts`: Define all your request to the API you want to use.

{% hint style="info" %}
However, keep in mind that it's totally up to you to choose the structure you want for the files of your data provider. For this case, we found that it was better to split the Data Provider into these 3 files, but feel free to structure it however you like.

Only `index.ts` and `types.ts` are **mandatory**.
{% endhint %}

#### Let's start coding! 🧑‍💻

First, you need to define which query and which types you will use. From what we see in the Lens API docs, the `whoCollectedPublication()` query use and return some types, so you need to implement them in your data provider.

First, you can create a query that will use this type in the `query.ts` file:

```sh
# create the queries file
touch queries.ts
```

```typescript
// group-generators/helpers/data-providers/tutorial-lens/queries.ts

// import gql in order to have prettier written requests
import { gql } from "graphql-request";
// import your previously created types
import { GetWhoCollectedPublicationType } from "./types";
// import the GraphQLProvider class
import { GraphQLProvider } from "@group-generators/helpers/data-providers/graphql";

// function that contains the query
export const getWhoCollectedPublicationQuery = async (
  graphqlProvider: GraphQLProvider,
  publicationId: string,
  cursor: string
): Promise<GetWhoCollectedPublicationType> => {
  // query
  return graphqlProvider.query<GetWhoCollectedPublicationType>(
    gql`
      query whoCollectedPublication($request: WhoCollectedPublicationRequest!) {
        whoCollectedPublication(request: $request) {
          items {
            address
          }
          pageInfo {
            prev
            next
            totalCount
          }
        }
      }
    `,
    {
      // the request will fetch 50 data by 50
      request: {
        // the id of the Lens post
        publicationId: publicationId,
        // number of data to fetch
        limit: 50,
        // this parameter allows specifying from which index you decide to fetch the data
        // example: cursor 50 & limit 100 => you will fetch the [50;100] data
        ...(cursor ? { cursor } : {}),
      },
    }
  );
};
```

Once done, create the `types.ts` file:

```sh
# create the types definitions file
touch types.ts
```

Then define all the types you need when using the `whoCollectedPublication()` query:

```typescript
// group-generators/helpers/data-providers/tutorial-lens/types.ts

// this type informs us which parts of the set of elements we have fetched.
// it allows us to do pagination
export type PageInfo = {
  prev: string;
  next: string;
  totalCount: number;
};

export type Wallet = {
  address: string;
};

export type GetWhoCollectedPublicationType = {
  whoCollectedPublication: {
    items: Wallet[];
    pageInfo: PageInfo;
  };
};

// this type will be used in the index.ts file following the tutorial
export type PublicationId = {
  publicationId: string;
};
```

Because Lens API is a GraphQL API, you can use the GraphQL Data Provider already created in the Hub in order to create the query.

As you can see, this query allows fetching 50 users at a time. So you need to create a function that will iterate on this query in order to fetch all the users.

All of this occurs in the `index.ts` file:

```sh
# create the index file
touch index.ts
```

```typescript
// group-generators/helpers/data-providers/tutorial-lens/index.ts

// import the queries
import {
  getWhoCollectedPublicationQuery,
} from "./queries";
// import the types
import {
  GetWhoCollectedPublicationType,
  PublicationId,
  Wallet
} from "./types";
// import the GraphQL Provider class
import { GraphQLProvider } from "@group-generators/helpers/data-providers/graphql";
// Import the FetchedData type
import { FetchedData } from "topics/group";

export class TutorialLensProvider extends GraphQLProvider {
  constructor() {
    super({
      // url of the API
      url: "https://api.lens.dev",
    });
  }
  
  // method that create the fetchedData object
  public async getWhoCollectedPublication(
    publication: PublicationId
  ): Promise<FetchedData> {
    const dataProfiles: FetchedData = {};
    for await (const item of this._getWhoCollectedPublication(publication)) {
      dataProfiles[item.address] = 1;
    }
    return dataProfiles;
  }

  // method that iterate on the getWhoCollectedPublicationQuery
  private async *_getWhoCollectedPublication({
    publicationId,
  }: PublicationId): AsyncGenerator<Wallet, void, undefined> {
    let cursor = "";
    let lensCollectors: GetWhoCollectedPublicationType;
    // fetch data as long as there is data
    do {
      lensCollectors = await getWhoCollectedPublicationQuery(
        this,
        publicationId,
        cursor
      );
      yield* lensCollectors.whoCollectedPublication.items;
      cursor = lensCollectors.whoCollectedPublication.pageInfo.next;
    } while (lensCollectors.whoCollectedPublication.items.length > 0);
  }
}
```

{% hint style="warning" %}
It is very important to pass an object that contains all your argument as argument of the function that the Factory will use.

Indeed, when the Factory create the group using your data provider, it will use take all the arguments gave by the user as input and create an object from it. Then it will call your function with this object, like this:

```typescript
const lensProviderData0 = await lensProvider.getWhoCollectedPublication({
    publicationId: "0x26e5-0x09"
});
// for this case there is only 1 argument
```

That is why the argument of the function used by the Factory must be an object that contains all the possible arguments of your function.

For example in our case we needed to define this type:

```typescript
export type PublicationId = {
  publicationId: string;
  // you can add other arguments here
};
```
{% endhint %}

It is worth noting that the final variable you return at the end of your `getWhoCollectedPublication` function is of type `FetchedData`. This type is needed if you want to create [Group Generators](../../how-sismo-works/resources/sismo-hub/sismo-protocol-overview.md).

Indeed, a Group Generator returns a group that is composed of metadata:

```typescript
{
  name: "test",
  timestamp: context.timestamp,
  description: "This is a test group",
  specs: "",
  data: dataProfiles,
  valueType: ValueType.Score,
  tags: [],
}
```

The `data` field is where the group of accounts is stored, and its type is a `FetchedData`.

That's why [`FetchedData`](https://github.com/sismo-core/sismo-hub/blob/main/src/topics/group/group.types.ts) must be the format of the variable you return.

```typescript
export type FetchedData = {
    [address: string]: BigNumberish;
};
```

And here's an example of how a `FetchedData` object looks like:

```json
{
  "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045": "1",
  "0xF61CabBa1e6FC166A66bcA0fcaa83762EdB6D4Bd": "1",
  "0x2E21f5d32841cf8C7da805185A041400bF15f21A": "1",
}
```

{% hint style="info" %}
Here there is only "`0x123...`" type addresses but the `fetchedData` object can contains also:

* ENS: "vitalik.eth"
* Lens handle: "stani.lens"
* Twitter account: "twitter:Sismo\_eth"
* GitHub account: "github:leosayous21"
* Telegram account: "telegram:pelealexandru"
{% endhint %}

Finally, you need to reference your newly created Data Provider in the `data-provider` folder in order to use it anywhere in the Sismo Hub:

```typescript
// group-generators/helpers/data-providers/index.ts

...
import { TutorialLensProvider } from "./tutorial-lens";

export const dataProviders = {
  ...
  TutorialLensProvider,
};
```

Congrats 🎉 You've just created your first Data Provider!

But now that you have finished, you probably want to test it right?

To do this you will have to create a group (only for testing) and import your Data Provider into it, so you can try your request. If you don't know how to create a group, check out the group tutorial [**here**](create-your-group-generator.md).

### Make your Data Provider usable by anyone on the Factory

Okay, so now that your data provider is finished and tested, it can be used by all devs in the Sismo Hub. The next step is making it available to anyone in the [Factory](https://factory.sismo.io/create-badge). Pretty exciting, isn't it? 🤩

To do so you need to create an `interface-schema.json` file that contains all the information the Factory needs to use your function and display relevant information.

```sh
# create the interface-schema file
touch interface-schema.json
```

```json
// group-generators/helpers/data-providers/tutorial-lens/interface-schema.json

{
  "name": "Tutorial Lens",
  "iconUrl": "",
  "providerClassName": "TutorialLensProvider",
  "functions": [
    {
      "name": "Get collectors of",
      "functionName": "getWhoCollectedPublication",
      "countFunctionName": "getPublicationCollectorsCount",
      "description": "Returns all collectors of a specific publication",
      "args": [
        {
          "name": "publication identifier",
          "argName": "publicationId",
          "type": "string",
          "example": "0x26e5-0x03",
          "description": "A Lens publication identifier in the following format {profileId}-{publicationId}. https://docs.lens.xyz/docs/get-publication"
        }
      ]
    }
  ]
}
```

{% hint style="info" %}
If you want more info on this schema, check this [section](../../how-sismo-works/resources/sismo-hub/data-providers.md#make-your-data-provider-usable-in-the-sismo-factory) on the Data Provider page.
{% endhint %}

Then you have to reference your schema:

<pre class="language-typescript"><code class="lang-typescript">// group-generators/helpers/data-providers/index.ts

...
import tutorialLensProviderSchema from "./tutorial-lens/interface-schema.json";

export const dataProvidersInterfacesSchemas = [
  ...
<strong>  tutorialLensProviderSchema,
</strong>];
</code></pre>

You're almost finished! 😉

As you may have noticed, in the `interface-schema.json` there is a `countFunctionName.` This is called a count function. Count functions allow the Factory to know how many accounts a request will fetch and thus to know if the data that the user wants to fetch exists.

Let's create it:

```typescript
// group-generators/helpers/data-providers/tutorial-lens/index.ts

...  
  public async getPublicationCollectorsCount(
    publication: PublicationId
  ): Promise<number> {
    const publicationStats = await getPublicationStatsQuery(
      this,
      publication.publicationId
    );
    return publicationStats.publication.stats.totalAmountOfCollects;
  }
```

```typescript
// group-generators/helpers/data-providers/tutorial-lens/queries.ts

...
export const getPublicationStatsQuery = async (
  graphqlProvider: GraphQLProvider,
  publicationId: string
): Promise<GetPublicationStatsType> => {
  return graphqlProvider.query<GetPublicationStatsType>(
    gql`
      query PublicationStats($request: PublicationQueryRequest!) {
        publication(request: $request) {
          ... on Post {
            stats {
              totalAmountOfCollects
            }
          }
          ... on Comment {
            stats {
              totalAmountOfCollects
            }
          }
          ... on Mirror {
            stats {
              totalAmountOfCollects
            }
          }
        }
      }
    `,
    {
      request: {
        publicationId: publicationId,
      },
    }
  );
};
```

```typescript
// group-generators/helpers/data-providers/tutorial-lens/types.ts

...
export type GetWhoMirroredPublicationType = {
  profiles: {
    items: ProfileType[];
    pageInfo: PageInfo;
  };
};
```

This count function (`getPublicationCollectorsCount`) will return the number of collectors for a specific Lens post. It allows the Factory to verify if the `publicationId` given by the user is correct but also to display the number of accounts the query will fetch on the website.

Finally, you have to reference your count function in the `dataProvidersAPIEndpoints`:

```typescript
// group-generators/helpers/data-providers/index.ts

export const dataProvidersAPIEndpoints = {
  ...
  // add your data provider to the /data-provider-interfaces end of the Sismo Hub API
  TutorialLensProvider: {
    // add your count function to your data provider on the Sismo Hub API
    getPublicationCollectorsCount: async (_: any) =>
      new TutorialLensProvider().getPublicationCollectorsCount(_),
  },
};
```

By adding this interface file and the count function, you just allowed the factory to:

* Fetch information about your newly created Data Provider. It can be now displayed (<mark style="color:red;">red box</mark>) and used for Factory users when adding eligible accounts.
* Call the right count function (regarding the user selection).

<figure><img src="../../.gitbook/assets/Capture d’écran 2023-03-02 à 11.14.44 (1).png" alt=""><figcaption><p>Data Provider selection</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/Capture d’écran 2023-03-02 à 11.26.10.png" alt=""><figcaption><p>Run the count function</p></figcaption></figure>

If you wish to choose the collectors for a Lens post, you would need to input the post's ID. After clicking the 'Add' button, the Factory will invoke your count function to verify the post's validity and display the number of eligible accounts at the bottom of the screen (see the example above).

You have finally set up your Data Provider for the Factory, congrats! 🎉

For our case, we use the Lens API which doesn't require any access token. However, for many APIs, that's not the case, so if you want to set an access token for the API you want to use, you need to create an environment variable. Here is how to do it:

```sh
# you are in sismo-hub root
cp .example.env .env
```

Then you can add your own access token by adding a new line to this file:

```properties
# in the .env file
export YOUR_DATA_PROVIDER_API_KEY="<API_KEY>"
```

And you need to export the variable in order for you to use it locally on your computer:

```sh
source .env
```

You are now able to use it on the Sismo Hub, here is the syntax:

```typescript
process.env.YOUR_DATA_PROVIDER_API_KEY
```

> NB: To be able to use the API key in the Sismo Hub when your badge is deployed, please contact us in **#dev-support** ([Sismo Discord](https://discord.gg/sismo)) after creating your PR so we can add your access token to our infrastructure.

### See your work integrated on Sismo

Your Data Provider is now ready, so it's time to push it into prod!

You don't have to store or launch anything on your side. You will just have to add [a pull request to the Sismo Hub](https://github.com/sismo-core/sismo-hub/pulls) to see your Data Provider live! 😁

First, you will have to [fork](https://docs.github.com/en/get-started/quickstart/fork-a-repo) the [Sismo Hub repository](https://github.com/sismo-core/sismo-hub):

<figure><img src="../../.gitbook/assets/Capture d’écran 2023-02-28 à 20.21.59 2.png" alt=""><figcaption><p>Fork Sismo Hub repository</p></figcaption></figure>

Then you will have to add a new remote to your repo:

```shell
git remote add fork <your_fork_repo_link.git>
```

You can also create a new branch for your work:

```sh
git checkout -b tutorial-lens-data-provider
```

In this tutorial you should have 5 different files changed:

* 4 files created:
  * `group-generators/helpers/data-providers/tutorial-lens/index.ts`
  * `group-generators/helpers/data-providers/tutorial-lens/queries.ts`
  * `group-generators/helpers/data-providers/tutorial-lens/types.ts`
  * `group-generators/helpers/data-providers/tutorial-lens/interface-schema.json`
* 1 file changed
  * `group-generators/helpers/data-providers/index.ts`

Next, add all your changes and push them to your fork:

```bash
# git add all your changes except the group you made to test your data provider
git add <all-your-files-except-the-test-group>
git commit -m "feat: add tutorial-lens data provider"
# if you created the branch tutorial-lens-data-provider
git push fork tutorial-lens-data-provider
# otherwise
git push
```

{% hint style="warning" %}
Don't forget to only add the files related to the Data Provider, and do not add the group you made for testing your Data Provider.
{% endhint %}

You will then see on your forked repository the following message, you can click on "Compare & pull request" to begin creating your pull request:

<figure><img src="../../.gitbook/assets/Capture d’écran 2023-02-28 à 19.35.52 2.png" alt=""><figcaption><p>Click on "Compare &#x26; pull request" to begin creating your PR</p></figcaption></figure>

You will finally see your branch being compared to the main branch of the [Sismo Hub](https://github.com/sismo-core/sismo-hub/) repository (<mark style="color:blue;">blue boxes</mark>). You can review your changes and add a meaningful title and comments, and when you are happy with your PR, click "Create Pull request". 😇

<figure><img src="../../.gitbook/assets/Capture d’écran 2023-02-28 à 19.47.50.png" alt=""><figcaption><p>Create your pull request</p></figcaption></figure>

For example, here is a valid pull request: [https://github.com/sismo-core/sismo-hub/pull/1407](https://github.com/sismo-core/sismo-hub/pull/1407)\
(The PR is closed because it's only used as an example for this tutorial)

{% hint style="info" %}
If your Data Provider requires an API key, contact the team on the [Sismo Discord](https://discord.gg/sismo) in **#dev-support** in order for us to make your API key usable in the main Sismo Hub and on the Factory.
{% endhint %}

When your PR is merged, you'll be able to see your Data Provider implemented in the Sismo Factory

You will also see it in the Sismo Hub API: [https://hub.sismo.io/data-provider-interfaces](https://hub.sismo.io/data-provider-interfaces)

Finally, your Data Provider will be available in the Sismo Hub and anyone will be able to use it to create groups. Great job! 💪

### Contribute to the Sismo Hub

You want to contribute, but you don't have any ideas for Data Providers to create? Check out the current [Sismo Hub GitHub's issues](https://github.com/sismo-core/sismo-hub/issues), as you will find some interesting ideas for Data Providers to implement.

If you have any questions or you need help regarding your Data Providers creation process, do not hesitate to join [Sismo Discord](https://discord.gg/sismo) and ask us in **#dev-support**. We will be glad to answer you 🤗
