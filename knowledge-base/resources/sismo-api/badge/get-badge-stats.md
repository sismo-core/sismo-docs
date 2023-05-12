---
description: Query toolkit to fetch statistics around badges
---

# Get Badge stats

### Query badge stats for one tokenId

{% tabs %}
{% tab title="Query" %}
```graphql
query getBadgeStatForId {
  badgeStat(network: polygon, id: 15151111) {
    id
    totalMintedBadges
    totalGasFee
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "badgeStat": {
      "id": "15151111",
      "totalMintedBadges": 16654,
      "totalGasFee": "890506561110463473345"
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Query badge stats for a list of token Ids

By default, they will be ordered by tokenId and consider all networks. But you can choose to order them by the total number of minted badges or gas fees spent for example and choose only one network.

{% tabs %}
{% tab title="Query" %}
```graphql
query getMostMintedBadges {
  badgeStats (first: 3, orderBy: totalMintedBadges){
    id
    totalMintedBadges
    totalGasFee
    network
  }
}
```
{% endtab %}

{% tab title="Query with a network" %}
```graphql
query getMostMintedBadgesOnPolygon {
  badgeStats (first: 3, network: polygon, orderBy: totalMintedBadges){
    id
    totalMintedBadges
    totalGasFee
    network
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "badgeStats": [
      {
        "id": "10000026",
        "totalMintedBadges": 20594,
        "totalGasFee": "933361448639077498528",
        "network": "polygon"
      },
      {
        "id": "15151111",
        "totalMintedBadges": 16654,
        "totalGasFee": "890506561110463473345",
        "network": "polygon"
      },
      {
        "id": "10000037",
        "totalMintedBadges": 7483,
        "totalGasFee": "356468540381938097496",
        "network": "polygon"
      }
    ]
  }
}
```
{% endtab %}
{% endtabs %}
