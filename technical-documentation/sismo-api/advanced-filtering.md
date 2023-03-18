---
description: Advanced usage of the API
---

# Advanced filtering

Before this, make sure you read the page about [common parameters](common-parameters.md).

### How to use `where`

The `where` filter lets you do some advanced queries. Here is how it works:

On `String` field

* `fieldname: "value"`
* `fieldname_not: "value"`
* `fieldname_in: ["value1", "value2"]`
* `fieldname_not_in: ["value1", "value2"]`

On `Int` , `BigInt` and `DateTime` field

* `fieldname: value`
* `fieldname_not: value`
* `fieldname_in: [value1, value2]`
* `fieldname_not_in: [value1, value2]`
* `fieldname_gt: value`
* `fieldname_lt: value`

On nested field

```graphql
query {
    badges(
        where: {
            tokenId: 15151111,
            balance_gt: 1,
            attestation: {
              timestamp_gt: 1600000000
            }
        }
    ) {
        balance
        metadata {
            name
        }
        owner {
            id
        }
    }
}
```
