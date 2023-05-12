---
description: Query toolkit to fetch badge metadata
---

# Get badges

### Query a badge

You can query a **Badge** thanks to its `id` or its `name`. The name argument is not case sensitive. it is worth noting that when querying a badge you can query the group snapshot used for the badge and the group metadata.

{% tabs %}
{% tab title="Query with id" %}
```graphql
query getBadgeWithId {
  badge (id: 15151111){
    attributes {
      trait_type
      value
    }
    availableNetworks
    description
    id
    image
    isCurated
    tokenId
    name
    tokenIdHex
    # here is the snapshot used for the badge
    snapshot {
      id
      dataUrl
      size
      timestamp
      valueDistribution {
        numberOfAccounts
        value
      }
      group {
        id
      }
    }
  }
}
```
{% endtab %}

{% tab title="Query with name" %}
```graphql
query getBadgeWithName {
  badge (name: "Sismo Contributor ZK Badge"){
    attributes {
      trait_type
      value
    }
    availableNetworks
    description
    id
    image
    isCurated
    tokenId
    name
    tokenIdHex
    snapshot {
      id
      dataUrl
      size
      timestamp
      valueDistribution {
        numberOfAccounts
        value
      }
      group {
        id
      }
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "badge": {
      "attributes": [
        {
          "trait_type": "PRIVACY",
          "value": "Very High"
        },
        {
          "trait_type": "TRUSTLESSNESS",
          "value": "High"
        },
        {
          "trait_type": "SYBIL_RESISTANCE",
          "value": "Medium"
        }
      ],
      "availableNetworks": [
        "polygon",
        "gnosis"
      ],
      "description": "ZK Badge owned by Sismo contributors. This Badge is used in Sismo Governance for contributors to voice their opinions.",
      "id": "15151111",
      "image": "https://sismo-prod-hub-static.s3.eu-west-1.amazonaws.com/badges/sismo_contributors.svg",
      "isCurated": true,
      "tokenId": "15151111",
      "name": "Sismo Contributor ZK Badge",
      "tokenIdHex": "0000000000000000000000000000000000000000000000000000000000e73007",
      "snapshot": {
        "id": "0xe9ed316946d3d98dfcd829a53ec9822e000000000000000000000000640f20ba",
        "dataUrl": "https://sismo-prod-hub-data.s3.eu-west-1.amazonaws.com/group-snapshot-store/0xe9ed316946d3d98dfcd829a53ec9822e/1678713018.json",
        "size": 9572,
        "timestamp": "1678713018",
        "valueDistribution": [
          {
            "numberOfAccounts": 6725,
            "value": "1"
          },
          {
            "numberOfAccounts": 2709,
            "value": "2"
          },
          {
            "numberOfAccounts": 138,
            "value": "3"
          }
        ],
        "group": {
          "id": "0xe9ed316946d3d98dfcd829a53ec9822e"
        }
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Query a list of badge

{% tabs %}
{% tab title="Query" %}
```graphql
query getBadges {
  badges (first: 10){
    attributes {
      trait_type
      value
    }
    availableNetworks
    description
    id
    image
    isCurated
    tokenId
    name
    tokenIdHex
    snapshot {
      dataUrl
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "badges": [
      {
        "attributes": [
          {
            "trait_type": "PRIVACY",
            "value": "Very High"
          },
          {
            "trait_type": "TRUSTLESSNESS",
            "value": "High"
          },
          {
            "trait_type": "SYBIL_RESISTANCE",
            "value": "Medium"
          }
        ],
        "availableNetworks": [
          "polygon"
        ],
        "description": "ZK Badge owned by Sismo Early users",
        "id": "0",
        "image": "https://sismo-prod-hub-static.s3.eu-west-1.amazonaws.com/badges/sismo_early_users.svg",
        "isCurated": true,
        "tokenId": "0",
        "name": "Sismo Early User ZK Badge",
        "tokenIdHex": "0000000000000000000000000000000000000000000000000000000000000000",
        // this badge was created in a smart contract
        // it explains why no snapshot is referenced
        "snapshot": null 
      },
      ...
      ...
      {
        "attributes": [],
        "availableNetworks": [
          "polygon"
        ],
        "description": "Be a verified human, member of the W3GS and participate in an event",
        "id": "12025886",
        "image": "https://sismo-prod-hub-static.s3.eu-west-1.amazonaws.com/badges/w3gs.svg",
        "isCurated": false,
        "tokenId": "12025886",
        "name": "W3GS ",
        "tokenIdHex": "0000000000000000000000000000000000000000000000000000000000b7801e",
        "snapshot": {
          "dataUrl": "https://sismo-prod-hub-data.s3.eu-west-1.amazonaws.com/group-snapshot-store/0x86897c9356ecabc9662254fe55067a10/1678715379.json"
        }
      }
    ]
  }
}

```
{% endtab %}
{% endtabs %}

### Query all curated badges

{% tabs %}
{% tab title="Query" %}
```graphql
query getBadges {
  badges (where: {isCurated: true}){
    attributes {
      trait_type
      value
    }
    availableNetworks
    description
    id
    image
    isCurated
    tokenId
    name
    tokenIdHex
    snapshot {
      dataUrl
        group {
          id
        }
      }
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "badges": [
      {
        "attributes": [
          {
            "trait_type": "PRIVACY",
            "value": "Very High"
          },
          {
            "trait_type": "TRUSTLESSNESS",
            "value": "High"
          },
          {
            "trait_type": "SYBIL_RESISTANCE",
            "value": "Medium"
          }
        ],
        "availableNetworks": [
          "polygon"
        ],
        "description": "ZK Badge owned by Sismo Early users",
        "id": "0",
        "image": "https://sismo-prod-hub-static.s3.eu-west-1.amazonaws.com/badges/sismo_early_users.svg",
        "isCurated": true,
        "tokenId": "0",
        "name": "Sismo Early User ZK Badge",
        "tokenIdHex": "0000000000000000000000000000000000000000000000000000000000000000",
        "snapshot": null
      },
      ...
      ...
      {
        "attributes": [
          {
            "trait_type": "PRIVACY",
            "value": "Very High"
          },
          {
            "trait_type": "TRUSTLESSNESS",
            "value": "High"
          },
          {
            "trait_type": "SYBIL_RESISTANCE",
            "value": "Medium"
          }
        ],
        "availableNetworks": [
          "polygon",
          "gnosis"
        ],
        "description": "ZK Badge owned by Sismo contributors. This Badge is used in Sismo Governance for contributors to voice their opinions.",
        "id": "15151111",
        "image": "https://sismo-prod-hub-static.s3.eu-west-1.amazonaws.com/badges/sismo_contributors.svg",
        "isCurated": true,
        "tokenId": "15151111",
        "name": "Sismo Contributor ZK Badge",
        "tokenIdHex": "0000000000000000000000000000000000000000000000000000000000e73007",
        "snapshot": {
          "dataUrl": "https://sismo-prod-hub-data.s3.eu-west-1.amazonaws.com/group-snapshot-store/0xe9ed316946d3d98dfcd829a53ec9822e/1678713018.json",
          "group": {
            "id": "0xe9ed316946d3d98dfcd829a53ec9822e"
          }
        }
      }
    ]
  }
}
```
{% endtab %}
{% endtabs %}
