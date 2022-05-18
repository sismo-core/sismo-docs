---
description: Where claims are proven and verified
---

# Claiming Protocol

A **Claiming Protocol** enables users to generate proofs for a defined set of claims (such as "I claim that I own an account that holds a Cryptopunk").

![](<../../.gitbook/assets/Sismo Claiming Scheme (3).png>)

Each claiming protocol is constituted of:&#x20;

* **Claiming Data**
  * A database which contains supported claims data _(e.g a merkle tree of cryptopunk owners)_
* **Proving Scheme** comprised of:
  * A prover: code that enables users generate proofs from the claiming data, _(e.g I own an address from the list of cryptopunk owners)_
  * A verifier: code that enables anyone to check proofs and validate claims

There are currently 2 Claiming protocols maintained by Sismo Genesis Team:

{% content-ref url="../attester/smps-simple-merkle-proving-scheme-attester.md" %}
[smps-simple-merkle-proving-scheme-attester.md](../attester/smps-simple-merkle-proving-scheme-attester.md)
{% endcontent-ref %}

{% content-ref url="../attester/zk-smps-zero-knowledge-simple-merkle-proving-scheme-attester.md" %}
[zk-smps-zero-knowledge-simple-merkle-proving-scheme-attester.md](../attester/zk-smps-zero-knowledge-simple-merkle-proving-scheme-attester.md)
{% endcontent-ref %}

Both consume the same public and auditable claiming data, stored as account merkle trees and maintained by Sismo Genesis Team.

SMCS is based on public merkle proofs while ZK-SMCS generate and verify merkle proofs in the form of SNARKs, guaranteeing privacy for the user making the claim.
