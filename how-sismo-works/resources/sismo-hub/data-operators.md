# Data Operators

#### Data Operators

Data Operators are helpers to create new Data Groups by combining existing ones.

Currently, there are 3 Data Operators:

* `Union`: join multiples groups of data together
* `Intersection`: take the intersection between 2 groups of data
* `Map`: map values of a group data with another value
* `Aggregation`: aggregate the value with multiples groups&#x20;

```typescript

const generator: GroupGenerator = {
  generationFrequency = GenerationFrequency.Weekly;
 
  generate: async (context: GenerationContext): Promise<GroupWithData[]> => {
    // retrieve the latest lens followers group
    const sismoFollowers = await groupStore.latest("sismo-lens-followers");
    // retrieve the latest masquerade followers group
    const masqueradeFollowers = await groupStore.latest(
      "masquerade-lens-followers"
    );
    
    // Use the Interesection data operator to create a new group
    const sismoAndMasqueradeFollowers = dataOperators.Intersection(
      await sismoFollowers.data(),
      await masqueradeFollowers.data()
    );
    
    return [
      {
        name: "sismo-and-masquerade-lens-followers",
        timestamp: context.timestamp,
        data: sismoAndMasqueradeFollowers,
        accountSources: [AccountSource.ETHEREUM],
        valueType: ValueType.Info,
        tags: [Tags.User, Tags.Lens, Tags.Web3Social],
      },
    ];
  },
};

export default generator;

```
