---
cover: .gitbook/assets/TWITTER BANNERvert_1500x500px.jpeg
coverY: 0
---

# Sismo Protocol Overview

The Sismo Protocol oversees the creation, update and deletion of attestations.

Before diving into the precise anatomy of an attestation and its creation process, we need to first introduce **Attesters** and **the** **Attestations Registry**. They are the core smart contracts of the Sismo Protocol, respectively in charge of **issuing** and **recording** attestations.

You can also look into the full picture of the protocol\[LINK TO MAIN SCHEME].

### Attesters and Attestations Registry

SCHEMA 1&#x20;



**The Attestations Registry** is the core smart contract, deployed on supported EVM chains. It stores user attestations in its **collections**. A collection regroups attestations backing the same claim, verified by the same attester (e.g Ethereum User in 2016, verified by ZK-SMPS Attester).

**Attesters** are the smart contracts that write attestations in the Attestations Registry. Each attester offers its **set of claims** and has its own **proving scheme** used to verify user claims.&#x20;

For instance, the ZK-SMPS Attester requires users to provide a ZK-SNARK proof to back a claim such as "Ethereum User in 2016". Once it has verified the proof, it issues the attestation which is stored it in the registry.

#### Authorized Attesters and Governance

Only Authorized Attesters can write in the Attestation Registry. They are attesters that were given write access on a specific range of attestations collections.&#x20;

The Sismo Governance is in charge of curating attesters so that the registry is filled with valuable attestations.&#x20;

The protocol could be thought as a city: only the best attesters should be authorized so it becomes attractive for others to join and record attestations there. Like a city, Sismo will invest in its accessibility (DevX, Standardization, UX) and infrastructure (Interroperability between attestations, Bridges)

```
```

{% content-ref url="broken-reference" %}
[Broken link](broken-reference)
{% endcontent-ref %}
