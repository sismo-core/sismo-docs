# Proving Schemes

A proving scheme is a circuit in which users establish ownership of pieces of personal data. Proving schemes consist of provers and verifiers. Provers generate proofs for user claims that are authenticated by verifiers integrated into applications.

When users connect to applications via Sismo Connect, they participate in a proving scheme to make claims about their Data Sources (e.g group memberships)

This [article](https://vitalik.ca/general/2022/06/15/using\_snarks.html) from Vitalik Buterin should give you a precise understanding of the intent behind Hydra and a general overview of the ZK SNARK concepts behind it.

## Hydra proving schemes

Currently, Sismo has two functional proving schemes deployed:

* [Hydra S1 proving scheme](hydra-s1.md) — [ZK Badges](broken-reference) (no longer maintained)
* [Hydra S2 proving scheme](hydra-s2.md) — [Sismo Connect](broken-reference)

Both of these proving schemes belong to the Hydra family, which consists of proving schemes using zero-knowledge proofs (ZKPs) and Sismo’s Commitment Mapper.

In Hydra proving schemes, users commit the Poseidon hash of a secret and receive an EdDSA signed receipt from the Commitment Mapper. A user’s secret and the signed receipt from the Commitment Mapper form the Hydra Proof of Ownership, which is verified inside a zk-SNARK circuit—preserving user privacy.

{% hint style="info" %}
The [Commitment Mapper ](../commitment-mapper.md)is an off-chain service that privately associates a commitment with a source account during proving schemes.
{% endhint %}
