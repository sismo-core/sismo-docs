# Proving Schemes

A proving scheme is a circuit in which users establish ownership of pieces of personal data known as Data Gems. Proving schemes consist of provers and verifiers. Provers generate proofs for user claims that are authenticated by verifiers integrated into applications.

When users connect to applications via Sismo Connect or mint Badges, they participate in a proving scheme to verify ownership of Data Gems (i.e, personal data) in their Data Vault.

This [article](https://vitalik.ca/general/2022/06/15/using\_snarks.html) from Vitalik Buterin should give you a precise understanding of the intent behind Hydra and a general overview of the ZK SNARK concepts behind it.

## Hydra proving schemes

Currently, Sismo has two functional proving schemes deployed:

* [Hydra S1 proving scheme](hydra-s1.md) — [ZK Badges](broken-reference)
* [Hydra S2 proving scheme](hydra-s2.md) — [Sismo Connect](../../../../what-is-sismo/discover-sismo-connect.md)

Both of these proving schemes belong to the Hydra family, which consists of proving schemes using zero-knowledge proofs (ZKPs) and Sismo’s Commitment Mapper.

In Hydra proving schemes, users commit the Poseidon hash of a secret and receive an EdDSA signed receipt from the Commitment Mapper. A user’s secret and the signed receipt from the Commitment Mapper form the Hydra Proof of Ownership, which is verified inside a zk-SNARK circuit—preserving user privacy.

{% hint style="info" %}
The [Commitment Mapper ](../commitment-mapper.md)is an off-chain service that privately associates a commitment with a source account during proving schemes.
{% endhint %}

## Pythia proving schemes

Sismo has conceptualized an additional family of proving schemes—known as Pythia. Although not yet deployed, Pythia proving schemes differ from their Hydra counterparts. Instead of using the Commitment Mapper, Pythia proving schemes use a Commitment Signer.

The following Pythia proving schemes have been proposed:

* [Pythia ZK proving scheme](pythia-zk-proving-scheme.md)
