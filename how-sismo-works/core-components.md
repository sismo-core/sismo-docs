---
description: An overview of how Sismo works.
---

# Core Components

Sismo enables communication between a user’s personal data and applications. The following components are fundamental in facilitating this communication:

* [Data Vault](technical-concepts/what-is-the-data-vault.md) — a local, sovereign and private identity aggregator
* [Data Sources](core-components.md#what-are-data-sources) — web2 or web3 accounts and other credentials aggregated in a user’s Data Vault
* [Data Groups](core-components.md#what-are-data-gems) — enable users to make claims about their Data Sources.
* [Proving schemes](technical-concepts/proving-schemes/) — cryptographic circuits that enable users to prove ownership of Data Sources and group membership of Data Sources
* [The Sismo Hub](core-components.md#what-is-the-sismo-hub) — Sismo’s data infrastructure used to create Data Groups, thus enabling users to make claims of group membership.

Sismo enables users to freely leverage Data Sources, bypassing the typical privacy limitations associated with web3 and the constraint of being confined to a single platform on web2.

## What are Data Sources?

Data Sources are the accounts, attestations and other credentials that users aggregate in their Data Vault. Users can prove ownership of Data Sources to authenticate themselves to applications via Sismo Connect.

Examples of Data Sources include:

* Web3 accounts (Ethereum addresses owned by a private key)
* Web2 accounts (GitHub, Twitter, Telegram, Discord)
* Attestations issued by governments, institutions, etc (coming soon)

## What are Data Groups?

```json
 {
  // Sismo Community Group made of multiple types of accounts
  // full group: https://sismo-prod-hub-data.s3.eu-west-1.amazonaws.com/group-snapshot-store/0xd630aa769278cacde879c5c0fe5d203c/1687260637.json
   ...
   "0xb08db4cd36b0309c069f515ced754205912a707a": "2", // contributor level 2
   "0xf1fc0b43f8fd0e8b8fce983732d48925148438c1": "2", // contributor level 2
   "twitter:CharlsCharls_": "3",                      // contributor level 3  
   "github:leosayous21": "2",                         // contributor level 2
   "github:mme022": "2",
   "dhadrien.eth": "2",
   "telegram:dhadrien": "3",
   ...
 }
```

Users with Sismo can make ZK Proof of group membership from Data Source.&#x20;

The owner of dhadrien.eth (part of Sismo Community Group Group) can generate a ZK Proof that

* He is a contributor of level higher than 1
* He is a contributor of level exactly equal to 3

Examples of Groups:

* Minters of  "[Stand with Crypto](https://nft.coinbase.com/collection/ethereum/0x9d90669665607f08005cae4a7098143f554c59ef)" NFT  ([Data Group](https://factory.sismo.io/groups-explorer?search=0xfae674b6cba3ff2f8ce2114defb200b1))
* Contributors to [Sismo Hub](https://github.com/sismo-core/sismo-hub) GitHub repo ([Data Group](https://factory.sismo.io/groups-explorer?search=0xda1c3726426d5639f4c6352c2c976b87))
* Gitcoin Passport Holders ([Data Group](https://factory.sismo.io/groups-explorer?search=0x1cde61966decb8600dfd0749bd371f12))
* [ENS DAO](https://docs.ens.domains/v/governance/) participants ([Data Group](https://factory.sismo.io/groups-explorer?search=0x85c7ee90829de70d0d51f52336ea4722))
* Sismo Community Group ([Data Group](https://factory.sismo.io/groups-explorer?search=0xd630aa769278cacde879c5c0fe5d203c))

## What are Proving Schemes?

Sismo enables users to prove ownership/ group membership of Data Sources by participating in proving schemes. Fundamentally, a proving scheme is a paired concept involving a prover and a verifier. The prover generates proofs while the verifier checks the validity of these proofs.

In a proving scheme, both the prover and the verifier require access to the same data—the source of truth from which proofs are generated, called Data Groups. For instance, to prove ownership of a specific NFT at a certain time, the prover and verifier must be synchronized on the same snapshot of NFT owners (i.e. a Data Group).

Sismo Connect is the developer-friendly layer on top of proving schemes. It enables apps to request and verify ownership/ gropu membership of Data Sources. Sismo Connect routes these requests to the associated proving scheme.

Currently, Sismo has the following proving schemes deploy_e_d:

* [Hydra S1 proving scheme](technical-concepts/proving-schemes/hydra-s1.md)
* [Hydra S2 proving scheme](technical-concepts/proving-schemes/hydra-s2.md)

These schemes are part of the Hydra family, a collection of proving schemes utilizing ZKPs in combination with Sismo’s [Commitment Mapper](technical-concepts/commitment-mapper.md).

## What is the Sismo Hub?

Proving schemes need access to data in a specific, agreed-upon format to function. The Sismo Hub is Sismo’s data infrastructure—open for anyone to use. It is the tool that standardizes raw data for the Sismo communication protocol, creating Data Groups.

Hydra proving schemes use onchain Merkle roots as sources of truth. The Sismo Hub generates Data Groups and transforms them into Merkle registry trees—publishing them onchain. Future proving schemes could utilize different sources of truth, such as public keys issued by centralized entities.

Data Groups essentially function as snapshots of a dataset at a given point in time. For example, to generate or verify the ownership proof of an NFT at a specific moment, both the prover and verifier need to align on a snapshot of NFT owners during that period.

{% hint style="info" %}
While Sismo manages the maintenance of Data Groups, anyone can create and register a Data Group via the Sismo Hub—either via a pull request or the [Sismo Factory](https://factory.sismo.io/) (a no-code interface).
{% endhint %}

In addition to Data Groups, the Sismo Hub manages the creation and maintenance of Data Providers. They allow for the programmatic updates of Data Groups, further ensuring that provers and verifiers always work with the most relevant data.
