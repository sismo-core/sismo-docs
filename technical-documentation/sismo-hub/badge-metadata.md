# Badge Metadata

All Badges metadata for the \`HydraS1AccountboundAttester\` can be declared in 2 different files:

* `badges-metadata/main/hydra-s1-accountbound.ts` : for badges created through the Sismo Hub
* `badges-metadata/main/factory/hydra-s1-accountbound-factory-badges.ts` : for badges created through the [Sismo Factory](https://factory.sismo.io/).

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
      groupSnapshot: {
        groupName: "ethereum-power-users",
      },
      publicContacts: [
        {
          type: "twitter",
          contact: "@sismo_eth",
        },
      ],
      links: []
    },
  ],
};
```

{% hint style="info" %}
Badge SVG files are saved in the `/static/badges` folder
{% endhint %}

This will automatically deploy the associated metadata for the badge&#x20;

example here: [https://hub.sismo.io/badges/polygon/0000000000000000000000000000000000000000000000000000000000989684.json](https://hub.sismo.io/badges/polygon/0000000000000000000000000000000000000000000000000000000000989684.json)
