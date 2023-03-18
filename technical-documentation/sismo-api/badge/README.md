---
description: Metadata of a Minted Badge
---

# Badge

You can use this query to learn more about the **Badge** type of the Sismo API.

```graphql
query {
  __type(name: "Badge") {
    name
    kind
    description
    fields {
      name
      description
    }
  }
}
```

The **Badge** type regroups all the metadata related to a specific badge. It features its off-chain/on-chain metadata (ERC1155 metadata, eligibility descriptions, curated attributes etc..).
