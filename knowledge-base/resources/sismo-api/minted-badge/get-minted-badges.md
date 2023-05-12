# Get minted badges

### Check if an account holds a badge on a specific network

You will need to specify an id for the minted badge that follows this structure:\
`<tokenId-network-ownerId>`

{% tabs %}
{% tab title="Query" %}
```graphql
query getMintedBadgeWithId {
  mintedBadge(
    # id -> <tokenId-network-ownerId>
    id: "10000026-polygon-0xf61cabba1e6fc166a66bca0fcaa83762edb6d4bd", 
  ) {
    id
    level
    network
    owner {
      id
    }
    badge {
      tokenId
    }
    issuer
    mintedAt
    transaction {
      id
    }
    rawAttestation {
      id
      extraData
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "mintedBadge": {
      "id": "10000026-polygon-0xf61cabba1e6fc166a66bca0fcaa83762edb6d4bd",
      "level": "1",
      "network": "polygon",
      "owner": {
        "id": "0xf61cabba1e6fc166a66bca0fcaa83762edb6d4bd"
      },
      "badge": {
        "tokenId": "10000026"
      },
      "issuer": "0x10b27d9efa4a1b65412188b6f4f29e64cf5e0146",
      "mintedAt": "1664364231",
      "transaction": {
        "id": "0x7a127a29502c69e224ef30bd610d908bfa34f2dfdd22e85458dcd65d6366a1d2"
      },
      "rawAttestation": {
        "id": "10000026-polygon-0xf61cabba1e6fc166a66bca0fcaa83762edb6d4bd",
        "extraData": ""
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

If the account does not hold this badge, you will encounter this error message:

{% code overflow="wrap" %}
```json
"Minted badge with tokenId 10000000 not found on network polygon for ownerId 0xf61cabba1e6fc166a66bca0fcaa83762edb6d4bd"
```
{% endcode %}

### Get all minted badges for an account

{% tabs %}
{% tab title="Query" %}
```graphql
query getAllMintedBadgesForAccount {
  mintedBadges (where: {owner: "0xf61cabba1e6fc166a66bca0fcaa83762edb6d4bd"}
) {
    id
    level
    network
    owner {
      id
    }
    badge {
      tokenId
    }
    issuer
    mintedAt
    transaction {
      id
    }
    rawAttestation {
      id
      extraData
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "mintedBadges": [
      {
        "id": "10000026-polygon-0xf61cabba1e6fc166a66bca0fcaa83762edb6d4bd",
        "level": "1",
        "network": "polygon",
        "owner": {
          "id": "0xf61cabba1e6fc166a66bca0fcaa83762edb6d4bd"
        },
        "badge": {
          "tokenId": "10000026"
        },
        "issuer": "0x10b27d9efa4a1b65412188b6f4f29e64cf5e0146",
        "mintedAt": "1664364231",
        "transaction": {
          "id": "0x7a127a29502c69e224ef30bd610d908bfa34f2dfdd22e85458dcd65d6366a1d2"
        },
        "rawAttestation": {
          "id": "10000026-polygon-0xf61cabba1e6fc166a66bca0fcaa83762edb6d4bd",
          "extraData": ""
        }
      },
      ...
      ...
      {
        "id": "12198586-gnosis-0xf61cabba1e6fc166a66bca0fcaa83762edb6d4bd",
        "level": "1",
        "network": "gnosis",
        "owner": {
          "id": "0xf61cabba1e6fc166a66bca0fcaa83762edb6d4bd"
        },
        "badge": {
          "tokenId": "12198586"
        },
        "issuer": "0x0a764805ad76a740d46c81c9a8978790c227084c",
        "mintedAt": "1675874365",
        "transaction": {
          "id": "0xce1457a63fbfb773358142d9b443abece3188724074c1d99b3fdc47a070ef750"
        },
        "rawAttestation": {
          "id": "12198586-gnosis-0xf61cabba1e6fc166a66bca0fcaa83762edb6d4bd",
          "extraData": "0000000000000000000000000000000000000000000000000000000000000040000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000200db2b0d0d787fe26260315e190ac2a3f24d71c99576e1a91deae76b79095a279"
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
query getAllMintedBadgesOnPolygonWithLevelAbove2ForAccount {
  mintedBadges (
  network: polygon, 
  where: {owner: "0xf61cabba1e6fc166a66bca0fcaa83762edb6d4bd", level_gte: 2,},
  orderBy: mintedAt,
  first: 5
) {
  # properties you want
}
```

### Get all minted badges

By default, this query will return the last 1000 badges minted on all networks. You obviously can choose a particular network for this query if you want, and also paginate with `skip` and `first` arguments.

{% tabs %}
{% tab title="Query" %}
```graphql
query getAllMintedBadges {
  mintedBadges {
  id
    level
    network
    issuer
    mintedAt
    badge {
      tokenId
    }
    rawAttestation {
      id
      extraData
    }
    transaction {
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
    "mintedBadges": [
      {
        "id": "10000070-gnosis-0xbbefc5bbac81c5e2b863d13dbcb374ea2ad5818a",
        "level": "1",
        "network": "gnosis",
        "issuer": "0x0a764805ad76a740d46c81c9a8978790c227084c",
        "mintedAt": "1677755490",
        "badge": {
          "tokenId": "10000070"
        },
        "rawAttestation": {
          "id": "10000070-gnosis-0xbbefc5bbac81c5e2b863d13dbcb374ea2ad5818a",
          "extraData": "0000000000000000000000000000000000000000000000000000000000000040000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000201c97f0cae4704042f73823a5303a3302e2f3373a01eadb6588f4910c26e9ea4e"
        },
        "transaction": {
          "id": "0x00e191e28411d9cbb4f3b3a20b7715b9fe991251659fadff8a0e7163963a196e"
        }
      },
      ...
      ...
      {
        "id": "12165660-gnosis-0xc80703c13de15b73b2e367d8aabb1b0db3922dfa",
        "level": "1",
        "network": "gnosis",
        "issuer": "0x0a764805ad76a740d46c81c9a8978790c227084c",
        "mintedAt": "1677755285",
        "badge": {
          "tokenId": "12165660"
        },
        "rawAttestation": {
          "id": "12165660-gnosis-0xc80703c13de15b73b2e367d8aabb1b0db3922dfa",
          "extraData": "000000000000000000000000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000020193f429047cf2b8af3f7090d2de660aaa45984840ef7937bad2d4bdc0c2b555c"
        },
        "transaction": {
          "id": "0x04949f7b7d5b272ebab03af694dfcfacd79df918378bc9931a5158a3f652082f"
        }
      }
    ]
  }
}
```
{% endtab %}
{% endtabs %}

```graphql
query getAllMintedBadgesWithFilters {
  mintedBadges (
    network: polygon, 
    first: 100, # select only 100 minted badges instead of 1000 (default)
    skip: 10000 # skip last 10000 minted badges
  ){
  # properties you want
}
```

