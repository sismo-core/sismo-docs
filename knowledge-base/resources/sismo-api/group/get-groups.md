---
description: Query toolkit to fetch group metadata and its snapshots
---

# Get groups

### Query a group

You can query a group by its id or its name. The name argument is not case sensitive, you can see all the groups created on the [Sismo Factory](https://factory.sismo.io/groups-explorer).

{% tabs %}
{% tab title="Query with an id" %}
```graphql
query getGroupFromId {
  group (id: "0x682544d549b8a461d7fe3e589846bb7b") {
    id
    name
    description
    specs
    generationFrequency
    latestSnapshot {
      id
      dataUrl
      size
      valueDistribution {
        value
        numberOfAccounts
      }
      timestamp
    }
    snapshots {
      id
      size
      timestamp
      valueDistribution {
        numberOfAccounts
        value
      }
    }
  }
}
```
{% endtab %}

{% tab title="Query with a name" %}
```graphql
query getGroupFromName{
  group (name: "proof-of-humanity") {
    id
    name
    description
    specs
    generationFrequency
    latestSnapshot {
      id
      dataUrl
      size
      valueDistribution {
        value
        numberOfAccounts
      }
      timestamp
    }
    snapshots {
      id
      size
      timestamp
      valueDistribution {
        numberOfAccounts
        value
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
    "group": {
      "id": "0x682544d549b8a461d7fe3e589846bb7b",
      "name": "proof-of-humanity",
      "description": "Prove you are a human with PoH",
      "specs": "Appear as a verified Proof of Humanity submission on the Proof of Humanity subgraph",
      "generationFrequency": "weekly",
      "latestSnapshot": {
        "id": "0x682544d549b8a461d7fe3e589846bb7b6c617465737400000000000000000000",
        "dataUrl": "https://sismo-prod-hub-data.s3.eu-west-1.amazonaws.com/group-snapshot-store/0x682544d549b8a461d7fe3e589846bb7b/1678111868.json",
        "size": 17054,
        "valueDistribution": [
          {
            "value": "1",
            "numberOfAccounts": 17014
          },
        ],
        "timestamp": "latest"
      },
      "snapshots": [
        {
          "id": "0x682544d549b8a461d7fe3e589846bb7b0000000000000000000000006405c984",
          "size": 17054,
          "timestamp": "1678100868",
          "valueDistribution": [
            {
              "numberOfAccounts": 17014,
              "value": "1"
            },
          ]
        },
        ...
        ...
        {
          "id": "0x682544d549b8a461d7fe3e589846bb7b000000000000000000000000631eff65",
          "size": 15711,
          "timestamp": "1662975845",
          "valueDistribution": [
            {
              "numberOfAccounts": 15711,
              "value": "1"
            }
          ]
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Query all groups

You can query all the groups created on Sismo with their latest snapshot (the last data for this group) with this simple query.

{% tabs %}
{% tab title="Query" %}
```graphql
query getAllGroups {
  groups {
    id
    generationFrequency
    description
    name
    specs
    latestSnapshot {
      dataUrl
      timestamp
      id
      size
      valueDistribution {
        numberOfAccounts
        value
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
    "groups": [
      {
        "id": "0x175a1e4c38a2d9bf13a143002be08468",
        "generationFrequency": "daily",
        "description": "Verify your e-mail address on Zohal's website",
        "name": "zohal-KYC",
        "specs": "Get your e-mail address verified",
        "latestSnapshot": {
          "dataUrl": "https://sismo-prod-hub-data.s3.eu-west-1.amazonaws.com/group-snapshot-store/0x175a1e4c38a2d9bf13a143002be08468/1678629415.json",
          "timestamp": "latest",
          "id": "0x175a1e4c38a2d9bf13a143002be084686c617465737400000000000000000000",
          "size": 24,
          "valueDistribution": [
            {
              "numberOfAccounts": 24,
              "value": "1"
            }
          ]
        }
      },
      ...
      ...
      {
        "id": "0x5b9d1b3cdabdd33363ccae4659b09eda",
        "generationFrequency": "once",
        "description": "",
        "name": "-lrcrypto",
        "specs": "",
        "latestSnapshot": {
          "dataUrl": "https://sismo-prod-hub-data.s3.eu-west-1.amazonaws.com/group-snapshot-store/0x5b9d1b3cdabdd33363ccae4659b09eda/1674390214.json",
          "timestamp": "latest",
          "id": "0x5b9d1b3cdabdd33363ccae4659b09eda6c617465737400000000000000000000",
          "size": 2,
          "valueDistribution": [
            {
              "numberOfAccounts": 2,
              "value": "1"
            }
          ]
        }
      }
    ]
  }
}
```
{% endtab %}
{% endtabs %}

