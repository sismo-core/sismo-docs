---
description: Managing the global record of Sismo attestations
---

# Sismo Attestation Protocol

**Sismo Attestation Protocol (SAP)** is the overarching protocol defining the set of rules linked to the creation, update and deletion of attestations stored in the [Sismo Attestations State ](sismo-attestations-state/)(SAS).

![](<../.gitbook/assets/Sismo Attestation Protocol (2).png>)

[Attestations](sismo-attestations-state/standard-attestation-format.md) are user claims (assertions of a statement or fact having occurred on a web3 account) that have been attested and recorded by authorized [Attesters](attester/) in the [Sismo Attestations State ](sismo-attestations-state/)(SAS). Only authorized [Attesters](attester/) have writing rights on the SAS.

Attesters receive claims from users, check them by verifying a proof generated through a dedicated Proving Scheme and by reading from the relevant Claims Datastore, and generate a corresponding attestation if the claim is valid.

The responsibility of authorizing new [Attesters](attester/) and the associated assignment of Shards (range of attestation collection slots in the SAS) will be progressively handed over from Sismo Genesis Team to the Sismo DAO once the protocol is live and new attester onboarding process is mature.
