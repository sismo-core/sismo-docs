---
description: Tokenized representation of an Attestation
---

# Minted Badge

You can use this query to learn more about the **MintedBadge** type of the Sismo API.

```graphql
query {
  __type(name: "MintedBadge") {
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

A **Minted Badge** is the tokenized representation of an **Attestation**. Any **Minted** **Badge** has a one-to-one relationship with an **Attestation**, as they serve the same purpose. The difference is that **Badges** are a stateless ERC1155 smart contract that forwards the result of the `balanceOf` method to the `AttestationRegistry`'s `getAttestationValue` method.\


As an integrator, you could quickly gate any of your service by looking at all the minted Badges hold by an **Account** on any network. You could also try to query if an **Account** holds a specific **Badge** or even fetch all minted **Badges** on Sismo in simple queries.&#x20;
