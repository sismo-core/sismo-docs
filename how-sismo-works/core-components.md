---
description: An overview of how Sismo works.
---

# Core Components

The Sismo protocol enables communication between a user’s personal data and applications. The following components are fundamental in facilitating this communication:

* [Data Vault](../welcome-to-sismo/what-is-the-data-vault.md) — a local, sovereign and private identity aggregator
* [Data Sources](core-components.md#what-are-data-sources) — web2 or web3 accounts and other credentials aggregated in a user’s Data Vault
* [Data Gems](core-components.md#what-are-data-gems) — provable granular pieces of sovereign data derived from Data Sources in a user’s Data Vault
* [Proving schemes](technical-concepts/proving-schemes/) — cryptographic circuits that enable users to prove ownership of Data Sources and Data Gems
* [The Sismo Hub](core-components.md#what-is-the-sismo-hub) — Sismo’s data infrastructure used to create Data Groups, thus defining new Data Gems

Sismo enables users to freely leverage Data Sources and Data Gems, bypassing the typical privacy limitations associated with web3 and the constraint of being confined to a single platform on web2.

## What are Data Sources?

Data Sources are the accounts, attestations and other credentials that users aggregate in their Data Vault. Users can prove ownership of Data Sources to authenticate themselves to applications via Sismo Connect.

Examples of Data Sources include:

* Web3 accounts (Ethereum addresses owned by a private key)
* Web2 accounts (GitHub, Twitter, Telegram, Discord)
* Attestations issued by governments, institutions, etc (coming soon)

## What are Data Gems and Data Groups?

Data Gems are the granular pieces of personal data that users selectively reveal to applications via Sismo Connect. They attest membership of a Data Source in a Data Group. A Data Group is a list of Data Sources with an associated value.

Examples of Groups:

* NFT Owners: number of NFT owners ([example](https://factory.sismo.io/groups-explorer?search=coinbase-shield-holder))
* GitHub contributors to a specific repository: number of commits ([example](https://factory.sismo.io/groups-explorer?search=sismo-github-contributors))
* Participants in a specific DAO: number of votes submitted ([example](https://factory.sismo.io/groups-explorer?search=ens-voters))

All Data Gems have the following characteristics:

* An owner - the Data Source
* A value - a number corresponding to a certain level of reputation
* A type - a unique ID used to identify the Data Gem

Examples of Data Gems include:

* NFT owners of a specific solution
* GitHub committers of a specific repository
* Participants in a specific DAO

## What are Proving Schemes?

Sismo enables users to prove ownership of Data Gems by participating in proving schemes. Fundamentally, a proving scheme is a paired concept involving a prover and a verifier. The prover generates proofs while the verifier checks the validity of these proofs.

In a proving scheme, both the prover and the verifier require access to the same data—the source of truth from which proofs are generated, called Data Groups. For instance, to prove ownership of a specific NFT at a certain time, the prover and verifier must be synchronized on the same snapshot of NFT owners (i.e. a Data Group).

Sismo Connect is the developer-friendly layer on top of proving schemes. It enables apps to request and verify ownership of data, either Data Sources or Data Gems. Sismo Connect routes these requests to the associated proving scheme.

Currently, Sismo has the following proving schemes deploy_e_d:

* [Hydra S1 proving scheme](technical-concepts/proving-schemes/hydra-s1.md)
* [Hydra S2 proving scheme](technical-concepts/proving-schemes/hydra-s2.md)

These schemes are part of the Hydra family, a collection of proving schemes utilizing ZKPs in combination with Sismo’s [Commitment Mapper](technical-concepts/commitment-mapper.md).

## What is the Sismo Hub?

Proving schemes need access to data in a specific, agreed-upon format to function. The Sismo Hub is Sismo’s data infrastructure—open for anyone to use. It is the tool that standardizes raw data for the Sismo communication protocol, creating Data Groups and defining new Data Gems.

Hydra proving schemes use onchain Merkle roots as sources of truth. The Sismo Hub generates Data Groups and transforms them into Merkle registry trees—publishing them onchain. Future proving schemes could utilize different sources of truth, such as public keys issued by centralized entities.

Data Groups essentially function as snapshots of a dataset at a given point in time. For example, to generate or verify the ownership proof of an NFT at a specific moment, both the prover and verifier need to align on a snapshot of NFT owners during that period.

{% hint style="info" %}
While Sismo manages the maintenance of Data Groups, anyone can create and register a Data Group via the Sismo Hub—either via a pull request or the [Sismo Factory](https://factory.sismo.io/) (a no-code interface).
{% endhint %}

In addition to Data Groups, the Sismo Hub manages the creation and maintenance of Data Providers. They allow for the programmatic updates of Data Groups, further ensuring that provers and verifiers always work with the most relevant data.

By organizing and anchoring data for proving schemes, the Sismo Hub plays a crucial role in facilitating the creation of Data Gems—giving application developers the power to request and verify ownership of them via Sismo Connect.
