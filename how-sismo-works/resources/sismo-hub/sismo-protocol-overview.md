---
cover: ../../../.gitbook/assets/TWITTER BANNERvert_1500x500px.jpeg
coverY: 0
---

# Group Generators

Groups are a reusable tool used by Sismo to generate available Data Groups for proving schemes.

You will find more information on what are groups in this section:

{% content-ref url="../../../knowledge-base/resources/sismo-hub/broken-reference/" %}
[broken-reference](../../../knowledge-base/resources/sismo-hub/broken-reference/)
{% endcontent-ref %}

Generating Data Groups and making them available for proving schemes requires some infrastructure. We have developed a repository, the [sismo-hub](https://github.com/sismo-core/sismo-hub), to let anyone propose new Data Groups and make them available for Hydra proving schemes with a simple PR.

You will be able to create your own group of accounts (Ethereum addresses, Github accounts and Twitter accounts) and then use it in a [Sismo Connect](broken-reference) app.

**Here is a complete tutorial describing all the group creation process steps:**

{% content-ref url="../../how-to-create-data-gems/create-your-group-generator.md" %}
[create-your-group-generator.md](../../how-to-create-data-gems/create-your-group-generator.md)
{% endcontent-ref %}

The following documentation aims to describe the code in a more theoretical way, we strongly recommend doing the tutorial to understand what the code below is for, especially for newcomers.

### Generate Function

```typescript
const generator: GroupGenerator = {
  generationFrequency: GenerationFrequency.Once,

  generate: async (context: GenerationContext): Promise<GroupWithData[]> => {
    // add you code here
    return [];
  },
};

export default generator;
```

A group generator is a tool that allows us to easily generate groups and store them in scalable infrastructure.

The **GroupGenerator** object is made of a generate function which will be executed at a specific `GenerationFrequency`.

Here the `GenerationFrequency` is `Once`. That means it will be executed just one time. It is an Enum that accepts `Daily`, `Weekly` or `Monthly` value.

Let's improve our group generator to return a very simple group:

```typescript
const generator: GroupGenerator = {
  generationFrequency: GenerationFrequency.Once,

  generate: async (context: GenerationContext): Promise<GroupWithData[]> => {
    return [
      {
        name: "my-simple-group",
        timestamp: context.timestamp,
        description: "description of my-simple-group"
        specs: "specs"
        data: {
          "0xF61CabBa1e6FC166A66bcA0fcaa83762EdB6D4Bd": 1,
          "0x8ab1760889f26cbbf33a75fd2cf1696bfccdc9e6": 1,
          "dhadrien.sismo.eth" : 1,
          "dhadrien.lens": 1,
          "github:leosayous21": 2,
          "github:dhadrien": 4,
          "twitter:dhadrien_": 3,
          "telegram:pelealexandru": 1
        },
        valueType: ValueType.Info,
        tags: [Tags.Mainnet, Tags.User],
      },
    ];
  },
};

export default generator;
```

In the above example, the group generator will create a group named `my-simple-group`. Its timestamp will correspond to the execution of the `generate` function.

This group, `my-simple-group`, contains 7 accounts, four Ethereum addresses (two addresses as you know them, one ENS handle and one Lens handle) which all have a value of 1, two GitHub accounts with values of 2 and 4 and one Twitter account with a value of 3 as you can see in the `data` field.

{% hint style="info" %}
* `Tags` are used to easily retrieve groups once they are generated.
{% endhint %}

Learn how to query data easily:

{% content-ref url="data-providers.md" %}
[data-providers.md](data-providers.md)
{% endcontent-ref %}

Learn how to combine groups easily:

{% content-ref url="data-operators.md" %}
[data-operators.md](data-operators.md)
{% endcontent-ref %}

### Generate your Group

You can find all the groups that can be generated in the `group-generators/generators` folder.\
This is where you need to reference your group to see it generated in the future.

```typescript
import ensVoters from "./ens-voters";
import ethereumDevelopers from "./ethereum-developers";
import ethereumMostTransactions from "./ethereum-most-transactions";
import ethereumPowerUsers from "./ethereum-power-users";
...
import sismoLensFollowers from "./sismo-lens-followers";
import sismoMasqueradeLensFollowers from "./sismo-masquerade-lens-followers";

import { GroupGeneratorsLibrary } from "topics/group-generator";

export const groupGenerators: GroupGeneratorsLibrary = {
  "ens-voters": ensVoters,
  "ethereum-developers": ethereumDevelopers,
  "ethereum-most-transactions": ethereumMostTransactions,
  "ethereum-power-users": ethereumPowerUsers,
  ...
  "sismo-lens-followers": sismoLensFollowers,
  "sismo-masquerade-lens-followers": sismoMasqueradeLensFollowers,
};
```

For more information or help, Join us on [Discord](https://discord.gg/xKTMeRUX6u).
