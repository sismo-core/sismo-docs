---
description: Where the user stores the data to back up his claims
---

# Source

A Source is an Ethereum or EVM chain address, imported by a user in his Sismo account, that is used as a source of data to attest the user meets the conditions required to generate an attestation or mint a specific badge.

If a claiming protocol specifies it, a source can be locked once it has been used once to generate an attestation. In that case, the source will not be useable again to generate the same attestation unless the user revokes that attestation.

When using private claiming protocols, these sources will not be linked to the destination where the attestations generated are hosted.
