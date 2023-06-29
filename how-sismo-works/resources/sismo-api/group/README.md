---
description: Source of Truth from which users can make statements about their data
---

# Group

You can use this query to learn more about the **Group** type of the Sismo API.

```graphql
query {
  __type(name: "Group") {
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

The **Group** type regroups all the metadata related to a specific group. It features its id, name, eligibility description and specifications. The data of the groups are stored in **GroupSnapshots** that are generated at a certain generation frequency.&#x20;
