---
description: Where the proofs
---

# Claiming Protocols

A **Claiming Protocol** enables users to generate proofs for a defined set of claims such as "Owner of Cryptopunk".&#x20;

Each claiming protocol is constituted of:&#x20;

* A database which contains supported claims data, e.g a merkle tree of cryptopunk owners.
* A proving scheme comprised of:
  * A prover: code that lets users generate proofs from the database, e.g I own an address from the list of cryptopunk owners.
  * A verifier: code that enables anyone to check proofs and validate claims

There are currently two Claiming Protocol maintained by Sismo Genesis Team:&#x20;

* Simple Merkle Claiming Protocol (SMCP)

{% content-ref url="smcp.md" %}
[smcp.md](smcp.md)
{% endcontent-ref %}

* Zero Knowledge Simple Merkle Claiming Protocol (ZK-SMCP)

{% content-ref url="zk-smcp.md" %}
[zk-smcp.md](zk-smcp.md)
{% endcontent-ref %}

Both consume the same public and auditable database of claims, stored as account merkle trees and maintained by Sismo Genesis Team.

SMCP is based on public merkle proofs while ZK-SMCP generate and verify merkle proofs in SNARKs, guaranteeing privacy for the user making the claim.
