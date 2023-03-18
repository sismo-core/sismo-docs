# Badge Metadata

Once you have created your group-generator, you need to specify the metadata for your Badge.

In the `bades-metadata/main/hydra-s1-accountbound.ts` file, all the badges metadata for the \`HydraS1AccountboundAttester\` are declared.&#x20;

```typescript

export const hydraS1AccountboundBadges: BadgesCollection = {
  collectionIdFirsts: {
    [Network.Polygon]: 10000001,
  },
  badges: [
    ...
    {
      internalCollectionId: 4,
      networks: [Network.Polygon, Network.Goerli, Network.Mumbai]
      name: "Ethereum Power User ZK Badge",
      description: "ZK Badge owned by the most active users on Ethereum",
      image: "ethereum_power_users.svg",
      groupGeneratorName: "ethereum-power-users",
      publicContacts: [
        {
          type: "twitter",
          contact: "@sismo_eth",
        },
      ],
      eligibility: {
        shortDescription:
          "Be part of the top 0.1% most active users on Ethereum",
        specification:
          "Be part of the top 50k accounts that sent the most transactions (token transfers excluded) on Ethereum between 2015 and December 31st 2016, or be part of the top 50k accounts between 2015 and December 31st 2017, or be part of the top 50k accounts between 2015 and December 31st 2018, or be part of the top 50k accounts between 2015 and December 31st 2019, or be part of the top 50k accounts between 2015 and December 31st 2020, or be part of the top 50k accounts between 2015 and December 31st 2021",
      },
      links: []
    },
  ],
};
```

{% hint style="info" %}
The svg file for your badge must be saved in the /static/badges folder
{% endhint %}

This will automatically deploy the associated metadata for the badge&#x20;

example here: [https://hub.sismo.io/badges/polygon/0000000000000000000000000000000000000000000000000000000000989684.json](https://hub.sismo.io/badges/polygon/0000000000000000000000000000000000000000000000000000000000989684.json)
