---
description: Query toolkit to fetch group snapshots
---

# Get group snapshots

Group snapshots store the data of a group at a specific timestamp. You can easily know the data of any group by knowing the `groupId` or the `groupName` and the associated `timestamp`.

### Get the latest snapshot&#x20;

You can query the latest snapshot by specifying the `groupId` or the `groupName`.

{% tabs %}
{% tab title="Query with groupId" %}
```graphql
query getSnapshot {
  groupSnapshot(
    groupId: "0x682544d549b8a461d7fe3e589846bb7b",
  ) {
    id
    dataUrl
    size
    timestamp
    valueDistribution {
      numberOfAccounts
      value
    }
  }
}
```
{% endtab %}

{% tab title="Query with groupName" %}
```graphql
query getSnapshot {
  groupSnapshot(
    groupName: "proof-of-humanity",
  ) {
    id
    dataUrl
    size
    timestamp
    valueDistribution {
      numberOfAccounts
      value
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "groupSnapshot": {
      "id": "0x682544d549b8a461d7fe3e589846bb7b6c617465737400000000000000000000",
      "dataUrl": "https://sismo-prod-hub-data.s3.eu-west-1.amazonaws.com/group-snapshot-store/0x682544d549b8a461d7fe3e589846bb7b/1678111868.json",
      "size": 16932,
      "timestamp": "latest",
      "valueDistribution": [
        {
          "numberOfAccounts": 16932,
          "value": "1"
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Get a snapshot at a specific timestamp

{% tabs %}
{% tab title="Query" %}
```graphql
query getSnapshotWithTimestamp {
  groupSnapshot(
    groupName: "proof-of-humanity",
    timestamp: "1675692588"
  ) {
    id
    dataUrl
    size
    timestamp
    valueDistribution {
      numberOfAccounts
      value
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "groupSnapshot": {
      "id": "0x682544d549b8a461d7fe3e589846bb7b00000000000000000000000063e10a2c",
      "dataUrl": "https://sismo-prod-hub-data.s3.eu-west-1.amazonaws.com/group-snapshot-store/0x682544d549b8a461d7fe3e589846bb7b/1675692588.json",
      "size": 16834,
      "timestamp": "1675692588",
      "valueDistribution": [
        {
          "numberOfAccounts": 16834,
          "value": "1"
        }
      ]
    }
  }
}
```
{% endtab %}
{% endtabs %}
