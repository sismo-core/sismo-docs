# Data Providers

## Use a Data Provider

In a `GroupGenerator`, users can query data. We have developed `dataProviders` to facilitate querying. We currently support the following `dataProviders`:

* `AlchemyProvider`: allows retrieving all holders of a specific NFT (ERC721 and ERC1155) on Ethereum mainnet
* `AnkrProvider`: allows all holders of a specific token (ERC20 and ERC721) on specific network
* `AttestationStationProvider`: allows you to query any attestation on Optimism's AttestationStation using a [Subgraph](https://thegraph.com/hosted-service/subgraph/wslyvh/optimism-atst)
* `BigQueryProvider`: allows launching arbitrary BigQuery queries on the Ethereum mainnet and Polygon dataset
* `DegenScoreProvider`: allows retrieving all holders of a beacon above a min score and with a specific trait
* `GithubProvider`: allows you to quickly retrieve user information from GitHub (commiters, organization members, stargazers...)
* `GitPoapProvider`: allows retrieving all Ethereum Addresses which have received a GitPoap with a given eventId
* `GraphQLProvider`: allows launching arbitrary graphQL queries
* `GuildProvider`: allows easily retrieving data from Guild.xyz
* `HiveProvider`: allows you to easily retrieve data about Twitter influencers
* `LensProvider`: allows retrieving social information on Lens Protocol (profiles, followers...)
* `PoapSubgraphProvider`: allows retrieving all attendees of any events
* `RestProvider`: allows launching arbitrary REST queries
* `SafeProvider`: allows retrieving all owners of given Safe wallet on the Ethereum mainnet.
* `SnapshotProvider`: allows querying all voters for a proposal or for an entire space
* `SubgraphHostedServiceProvider`: allows launching arbitrary queries on any subGraph
* `TransposeProvider`: allows you to launch arbitrary SQL requests on the Ethereum mainnet with [Transpose](https://www.transpose.io/) (token holders, smart contracts interactions...)
* `WiwBadgeProvider`: allows you to query WIW badge holders

Let's use the `SnapshotProvider` provider to create your group:

```typescript

const generator: GroupGenerator = {
  generationFrequency: GenerationFrequency.Weekly,

  generate: async (context: GenerationContext): Promise<GroupWithData[]> => {
    // Instantiate your snapshot provider
    const snapshotProvider = new dataProviders.SnapshotProvider();
    // Query all voters
    const voters = await snapshotProvider.queryAllVoters({
      space: "ens.eth",
    });
    return [
      {
        name: "ens-voters",
        timestamp: context.timestamp,
        data: voters,
        valueType: ValueType.Info,
        tags: [Tags.Mainnet, Tags.Vote, Tags.User],
      },
    ];
  },
};

export default generator;
```

The above information displays how to use data from external sources. Below, you can see how to combine groups with each other using [Data Operators](data-operators.md).

**You can find here a complete tutorial describing all the Data Provider creation process steps:**

{% content-ref url="../../../data-groups/data-groups-and-creation/create-your-data-provider.md" %}
[create-your-data-provider.md](../../../data-groups/data-groups-and-creation/create-your-data-provider.md)
{% endcontent-ref %}

## Create a new Data Provider

If you want to create a group by fetching accounts from an API for example, you can of course do it manually and put the hardcoded list of addresses in your group. But now if you want your group to be regularly updated (monthly, weekly, ord daily), you'll also have to manually update the group monthly, weekly or daily.

This is where Data Providers come in, they will allow you to fetch data (addresses or web2 accounts) in order to update your group.

Moreover creating your own Data Provider allow you or anyone to use it in the future to create a group.

### Setup the Data Provider

First, you'll need to create a new directory with the name of your data provider here: `./group-generators/helpers/data-providers/<DATA_PROVIDER_NAME>`

In this directory, you will have to create 2 different files:

* `index.ts` : It defines the API you want to use and the queries you want to add.
* `interface-schema.json` : This file is very important because it will allow you to make your Data Provider usable in the [Sismo Factory](https://factory.sismo.io/).

### Make your Data Provider usable in the [Sismo Factory](https://factory.sismo.io/)

Building a Data Provider is good, but making it usable for everyone in the [Sismo Factory](https://factory.sismo.io/) is even better 🙌

Here's where the different Data Providers elements will be displayed on the Factory:

<figure><img src="../../../.gitbook/assets/Capture d’écran 2023-02-14 à 17.28.37 4.png" alt=""><figcaption></figcaption></figure>

And here is how to setup this:

* First, create the `interface-schema.json` file with this format:

```json
{
  "name": "<DATA_PROVIDER_NAME>", // the name displayed in the factory
  "iconUrl": "", // currently not implemented in the factory
  "providerClassName": "<DATA_PROVIDER_CLASS_NAME>", // the real name of the class
  "functions": [ // define all the functions
    {
      "name": "<FUNCTION_NAME>", // name displayed
      "functionName": "<FUNCTION_NAME>", // real name
      "countFunctionName": "<FUNCTION_COUNT_NAME>",
      "description": "<FUNCTION_DESCRIPTION>",
      "args": [ // define all the arguments
        {
          "name": "profile identifier", // name displayed
          "argName": "profileId", // real argument name
          "type": "string",
          "example": "sismo.lens | 0x26e5 | sismo.eth | 0xB0A179C459484885D1875009110F3cE3064867B9",
          "description": "A Lens profile identifier that could be either of a Lens handle, a Lens profile Id, an ENS or an Ethereum address"
        }
      ]
    },
    ...
  ]
}
```

Now, go to `group-generators/helpers/data-providers/index.ts` file:

* Import your schema:

```typescript
import <DATA_PROVIDER_NAME>InterfaceSchema from "./<DATA_PROVIDER_NAME>/interface-schema.json";
```

* Add it to the `dataProvidersInterfacesSchemas`:

```typescript
export const dataProvidersInterfacesSchemas = [
  ...
  <DATA_PROVIDER_NAME>InterfaceSchema,
];
```

* Finally, as you can see, there is a black box at the bottom of the picture above. It allows users to see how many accounts your request will fetch (in real-time). It makes it easier for users to estimate their group size. So to set up this, add your _Count Functions_ in the `dataProvidersAPIEndpoints` object :

```typescript
export const dataProvidersAPIEndpoints = {
  ...
  <DATA_PROVIDER_CLASS_NAME>: {
    // add your count functions here
  },
};
```

You can explore the different Data Providers `index.ts` files to see some _Count Functions_ examples.

Do not hesitate to explore the different existing Data Providers and consider them as models. It will surely help you with building yours! 🙌

In addition, if you want to create a Data Provider that uses a GraphQL or a REST API, you can use the already existing GraphQL and REST Data Providers.

Want to contribute but don't have any ideas of Data Providers to add? Check out the Sismo GitHub's current issues [https://github.com/sismo-core/sismo-hub/issues](https://github.com/sismo-core/sismo-hub/issues) and our [Contributing guide](https://github.com/sismo-core/sismo-hub/blob/main/CONTRIBUTING.md).

If you have any questions or you need help regarding Data Providers, do not hesitate to join our [Discord](https://discord.gg/sismo) and ask us in **#dev-support**. We will be glad to answer you 🤗
