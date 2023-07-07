# Proving Schemes

## Prove & Verify

Sismo Connect technically let users participate in [proving schemes](../#what-are-proving-schemes) to make claims from their Data Sources.

{% hint style="info" %}
A proving scheme is a cryptographic method that allows one party (the prover) to prove to another party (the verifier) that a certain statement is true without revealing how it is true—ensuring privacy.
{% endhint %}

<figure><img src="../../../.gitbook/assets/Selective Disclosure (2).png" alt=""><figcaption></figcaption></figure>

The Data Vault includes the provers, enabling users to generate zero-knowledge proofs (ZKPs) from their Data Sources (proof of ownership/ proof of inclusion in a Data Group). The ZKP is then verified by the application (in a smart contract for onchain apps, in a backend of offchain apps, in a browser/client for p2p apps)

Sismo Connect is the developper experience (devX) layer on top of these proving schemes so developer do not have to think about them

Sismo enables users to prove ownership/ group membership of Data Sources by participating in proving schemes. Fundamentally, a proving scheme is a paired concept involving a prover and a verifier. The prover generates proofs while the verifier checks the validity of these proofs.

In a proving scheme, both the prover and the verifier require access to the same data—the source of truth from which proofs are generated, called Data Groups. For instance, to prove ownership of a specific NFT at a certain time, the prover and verifier must be synchronized on the same snapshot of NFT owners (i.e. a Data Group).

Sismo Connect is the developer-friendly layer on top of proving schemes. It enables apps to request and verify ownership/ gropu membership of Data Sources. Sismo Connect routes these requests to the associated proving scheme.

Currently, Sismo has the following proving schemes deploy_e_d:

* [Hydra S1 proving scheme](hydra-s1.md)
* [Hydra S2 proving scheme](hydra-s2.md)

These schemes are part of the Hydra family, a collection of proving schemes utilizing ZKPs in combination with Sismo’s [Commitment Mapper](../../technical-concepts/commitment-mapper.md).



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
The [Commitment Mapper ](../../technical-concepts/commitment-mapper.md)is an off-chain service that privately associates a commitment with a source account during proving schemes.
{% endhint %}
