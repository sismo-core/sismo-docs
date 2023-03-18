---
description: Query toolkit to fetch attestations
---

# Get attestations

An **Attestation** is the on-chain representation of a zero-knowledge proof at a specific timestamp. Each attestation has an [issuer authorized](https://docs.sismo.io/sismo-docs/sismo-protocol/attestations-registry#authorized-issuers) by the Sismo Governance for a range of tokenIds. Each attestation recorded will generate a Badge with its own tokenId.

{% hint style="info" %}
The queries for **Attestations** are very close to the ones used for **Minted Badges** as they serve the same purpose. The difference is that **Badges** are a stateless ERC1155 smart contract that forwards the result of the `balanceOf` method to the `AttestationRegistry`'s `getAttestationValue` method.
{% endhint %}

### Check if an account has an attestation on a specific network

You will need to specify an id for the attestation that follows this structure:\
`<tokenId-network-ownerId>`

{% tabs %}
{% tab title="Query" %}
```graphql
query getAttestationWithId {
  attestation(id: "10000026-polygon-0xf61cabba1e6fc166a66bca0fcaa83762edb6d4bd") {
    id
    network
    recordedAt
    timestamp
    value
    issuer
    owner {
      id
    }
    transaction {
      id
    }
    mintedBadge {
      id
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "attestations": [
      {
        "id": "10000026-polygon-0x7bf49814598924b1ab2a33ad815054b6367cddd0",
        "network": "polygon"
      },
      {
        "id": "12430503-gnosis-0x1faa8a4fa9808ebea3dcef0a4947b6ccb91e0e77",
        "network": "gnosis"
      },
      {
        "id": "10000026-gnosis-0x7bf49814598924b1ab2a33ad815054b6367cddd0",
        "network": "gnosis"
      }
    ]
  }
}
```
{% endtab %}
{% endtabs %}

If the account does not have this attestation, you will encounter this error message:

{% code overflow="wrap" %}
```json
"Attestation for account 0xf61cabba1e6fc166a66bca0fcaa83762edb6d4bd and tokenId 10000026 not found on network polygon"
```
{% endcode %}

### Get all attestations for an account

{% tabs %}
{% tab title="Query" %}
```graphql
query getAllAttestationsForAccount {
  attestations(where: {owner: "0xf61cabba1e6fc166a66bca0fcaa83762edb6d4bd"} ) {
    id
    network
    recordedAt
    timestamp
    value
    issuer
    owner {
      id
    }
    transaction {
      id
    }
    mintedBadge {
      id
    }
  }
} 
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "attestations": [
      {
        "id": "12820011-polygon-0xf61cabba1e6fc166a66bca0fcaa83762edb6d4bd",
        "network": "polygon",
        "recordedAt": "1676015700",
        "timestamp": "1675949258",
        "value": "1",
        "issuer": "0x10b27d9efa4a1b65412188b6f4f29e64cf5e0146",
        "owner": {
          "id": "0xf61cabba1e6fc166a66bca0fcaa83762edb6d4bd"
        },
        "transaction": {
          "id": "0xbd1ea8c195c3cabf8c12743f31c1a63d91d3880ba2b159cadc830aa2239157fa"
        },
        "mintedBadge": {
          "id": "12820011-polygon-0xf61cabba1e6fc166a66bca0fcaa83762edb6d4bd"
        }
      },
      ...
      ...
      {
        "id": "10000026-polygon-0xf61cabba1e6fc166a66bca0fcaa83762edb6d4bd",
        "network": "polygon",
        "recordedAt": "1664364231",
        "timestamp": "1664363276",
        "value": "1",
        "issuer": "0x10b27d9efa4a1b65412188b6f4f29e64cf5e0146",
        "owner": {
          "id": "0xf61cabba1e6fc166a66bca0fcaa83762edb6d4bd"
        },
        "transaction": {
          "id": "0x7a127a29502c69e224ef30bd610d908bfa34f2dfdd22e85458dcd65d6366a1d2"
        },
        "mintedBadge": {
          "id": "10000026-polygon-0xf61cabba1e6fc166a66bca0fcaa83762edb6d4bd"
        }
      }
    ]
  }
}
```
{% endtab %}
{% endtabs %}

You can also choose to add more filters or order in the query by specifying a specific network and more filters.&#x20;

```graphql
query getAllAttestationsOnPolygonWithValueAbove2ForAccount {
  attestations (
  network: polygon, 
  where: {owner: "0xf61cabba1e6fc166a66bca0fcaa83762edb6d4bd", value_gte: 2,},
  orderBy: createdAt,
  first: 5
) {
  # properties you want
}
```

### Get all attestations

By default, this query will return the last 1000 attestations on all networks. You obviously can choose a particular network for this query if you want, and also paginate with `skip` and `first` arguments.

{% tabs %}
{% tab title="Query" %}
```graphql
query getAllAttestations {
  attestations {
    id
    network
    recordedAt
    timestamp
    value
    issuer
    owner {
      id
    }
    transaction {
      id
    }
    mintedBadge {
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
    "attestations": [
      {
        "id": "10000070-gnosis-0xc12fcfdfd89929a009ec53f8c7f9c7baf5bfd5a1",
        "network": "gnosis",
        "recordedAt": "1677765775",
        "timestamp": "1677762621",
        "value": "1",
        "issuer": "0x0a764805ad76a740d46c81c9a8978790c227084c",
        "owner": {
          "id": "0xc12fcfdfd89929a009ec53f8c7f9c7baf5bfd5a1"
        },
        "transaction": {
          "id": "0x685ed724ce93f01f8b512a80f68b10881e89542c50feaf1610aecc25a75ece2c"
        },
        "mintedBadge": {
          "id": "10000070-gnosis-0xc12fcfdfd89929a009ec53f8c7f9c7baf5bfd5a1"
        }
      },
      ...
      ...
      {
        "id": "10000070-gnosis-0xc722b0a6407942c4a7633330cc81d2ad43d5f7cd",
        "network": "gnosis",
        "recordedAt": "1677765615",
        "timestamp": "1677762621",
        "value": "1",
        "issuer": "0x0a764805ad76a740d46c81c9a8978790c227084c",
        "owner": {
          "id": "0xc722b0a6407942c4a7633330cc81d2ad43d5f7cd"
        },
        "transaction": {
          "id": "0x001f53baa5fc22e977b4f15922ac15beb39a4bcc25a334ce580e0c296f93b614"
        },
        "mintedBadge": {
          "id": "10000070-gnosis-0xc722b0a6407942c4a7633330cc81d2ad43d5f7cd"
        }
      }
    ]
  }
}
```
{% endtab %}
{% endtabs %}

```graphql
query getAllAttestationsWithFilters {
  attestations (
    network: polygon, 
    first: 100, # select only 100 minted badges instead of 1000 (default)
    skip: 10000 # skip last 10000 minted badges
  ){
  # properties you want
}
```
