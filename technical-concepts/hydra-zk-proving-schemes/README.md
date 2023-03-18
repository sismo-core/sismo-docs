# Hydra ZK Proving Schemes

The Hydra family bundles proving schemes that use Hydra Delegated Proof of Ownership.&#x20;

{% hint style="info" %}
The following [article](https://vitalik.ca/general/2022/06/15/using\_snarks.html) from Vitalik Buterin should give you a precise understanding about the intent behind Hydra and a general overview of the ZK SNARK concepts behind it.
{% endhint %}

### Hydra Proving Scheme Family

The Hydra Family currently has three proving scheme members. A Proving Scheme has a **prover** that is **** able to generate proofs for a **verifier** that can verify their validity.

* Hydra-S1 Proving Scheme ([circuits](https://github.com/sismo-core/hydra-s1-zkps), [docs](hydra-s1.md)) (S1 Single Source. Version 1: one group membership verification)\
  Two attesters have implemented Hydra-S1 Proving Scheme to generate ZK Attestations:
  * [Hydra-S1 Simple Attester ](https://github.com/sismo-core/sismo-protocol/tree/main/contracts/attesters/hydra-s1)
  * [Hydra-S1 Soulbound Attester](https://github.com/sismo-core/sismo-protocol/tree/main/contracts/attesters/hydra-s1/variants)
* Hydra-S2 Proving Scheme (S2: Single Source. Version 2: multiple membership verifications)\
  Open-sourcing soon
* Hydra-M1 Proving Scheme (M1: Multi Source. Version 1)\
  Open-sourcing soon

### Hydra Proof Of Ownership

In order to use Hydra proving schemes, the user must interact with a trusted [Commitment Mapper](../commitment-mapper.md) from their account, and commit the Poseidon hash of a secret to receive the EdDSA signed receipt (mapping their account identifier with their commitment) from the commitment mapper.&#x20;

**The user secret and the commitment mapper's signed receipt form the Hydra Proof Of Ownership**

The user will then be able to use its Hydra Proof of Ownership in a SNARK to prove ownership of its account.

{% hint style="success" %}
Using the commitment mapper with the Poseidon Hash function and the EdDSA signature schemes makes the Delegated Proof of Ownership cheaply verifiable in a SNARK.
{% endhint %}

All Hydra Proving schemes use one or multiple Hydra Proof of Ownership verifications.
