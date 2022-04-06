---
description: Managing the global record of Sismo attestations
---

# Sismo Attestation Protocol

**Sismo Attestation Protocol (SAP)** is the overarching protocol defining the set of rules linked to the creation, update and deletion of attestations stored in the [Sismo Attestations State ](../architecture/sismo-attestations-state/)(SAS).

![](<../.gitbook/assets/Sismo Attestation Protocol.png>)

[Attestations](../architecture/sismo-attestations-state/standard-attestation-format.md) are user claims (assertions of a statement or fact having occurred on a web3 account) that have been attested and stored by Sismo Attestation Protocol in the [Sismo Attestations State ](../architecture/sismo-attestations-state/)(SAS). When a user makes a request for claim to the SAP by proving ownership of his web3 source accounts, his request for claim is routed to the relevant [Claiming Scheme](../architecture/claiming-scheme/) to generate a proof and this proof is verified by a specific [Attester](../architecture/attester.md) smart contract before being recorded in the SAS.

The responsibility of authorizing new [Claiming Schemes](../architecture/claiming-scheme/), [Attesters](../architecture/attester.md) and the associated assignment of Shards (range of attestation collection slots in the SAS) will be progressively handed over from Sismo Genesis Team to the Sismo DAO.
