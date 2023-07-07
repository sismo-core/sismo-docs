# Proving Schemes

## Prove & Verify

Sismo Connect technically let users participate in proving schemes to make claims from their Data Sources.

{% hint style="info" %}
A proving scheme is a cryptographic method that allows one party (the prover) to prove to another party (the verifier) that a certain statement is true without revealing how it is true—ensuring privacy.
{% endhint %}

<figure><img src="../../../.gitbook/assets/Selective Disclosure (2).png" alt=""><figcaption></figcaption></figure>

The Data Vault includes the provers, enabling users to generate zero-knowledge proofs (ZKPs) from their Data Sources (proof of ownership/ proof of inclusion in a Data Group). The ZKP is then verified by the application (in a smart contract for onchain apps, in a backend of offchain apps, in a browser/client for p2p apps)

## Proving Schemes

Fundamentally, a proving scheme is a paired concept involving a prover and a verifier. The prover generates proofs while the verifier checks the validity of these proofs.

In a proving scheme, both the prover and the verifier require access to the same data—the source of truth from which proofs are generated, called Data Groups. For instance, to prove ownership of a specific NFT at a certain time, the prover and verifier must be synchronized on the same snapshot of NFT owners (i.e. a Data Group).

Sismo Connect is the developer-friendly layer on top of proving schemes. It enables apps to request and verify ownership/ group membership of Data Sources. Sismo Connect routes these requests to the associated proving scheme.

Currently, Sismo has the following proving schemes deploy_e_d:

* [Hydra S1 proving scheme](hydra-s1.md)
* [Hydra S2 proving scheme](hydra-s2.md)

In Hydra proving schemes, users commit the Poseidon hash of a secret and receive an EdDSA signed receipt from the Commitment Mapper. A user’s secret and the signed receipt from the Commitment Mapper form the Hydra Proof of Ownership, which is verified inside a zk-SNARK circuit—preserving user privacy.

{% hint style="info" %}
The [Commitment Mapper ](../../technical-concepts/commitment-mapper.md)is an off-chain service that privately associates a commitment with a source account during proving schemes.
{% endhint %}
