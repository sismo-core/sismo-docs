# Common parameters

All our queries follow the same pattern.

#### Parameters when querying a list

All parameters are **optional**.

* **skip:** Where to start pagination. _Default: 0._
* **first**: The number of items to query. _Default: 100._
* **orderBy**: The key of the item used to sort them.
* **orderDirection**: asc or desc. _Default: asc._
* **where**: All fields can be used to operate [advance queries](common-parameters.md#advance-querying).

_Example: query the first 10 MintedBadges ordered by level_

```graphql
query {
  mintedBadges(first: 10, orderBy: level) {
    level
    badge {
      tokenId
    }
  }
}
```

#### Parameters when querying one item

Only the **id** is **mandatory**.

* **id**: The id of the item to query

_Example with the id "10000009-polygon-0xf61cabba1e6fc166a66bca0fcaa83762edb6d4bd"_

_In English, it would mean: "query the Minted Badge with the tokenId 10000009 for the address  `0xf61cabba1e6fc166a66bca0fcaa83762edb6d4bd` on Polygon."_

```graphql
query {
  mintedBadge(
    id: "10000026-polygon-0xf61cabba1e6fc166a66bca0fcaa83762edb6d4bd",
  ) {
    level
    badge {
      tokenId
    }
  }
}
```
