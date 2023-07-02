---
description: Sismo's open data infrastructure.
---

# How To Create Data Groups

Data Groups are created in the Sismo Hub and enable users to make claims of group memberships from their Data Sources.

The Sismo Hub is Sismo’s open data infrastructure. Anyone can use it to make data available to the Sismo protocol in an agreed-upon format. This data is used as a source of truth during proving schemes.

## What is a Data Group?

The Sismo Hub includes an onchain registry of Data Groups. Anyone can use the Sismo Hub to create a Data Group, either via a pull request or the [Sismo Factory](https://factory.sismo.io/) (a no-code interface). Fundamentally, Data Groups are lists of accounts (Ethereum, GitHub, Twitter and Telegram) with each a value.

Examples of Groups:

* Minters of  "[Stand with Crypto](https://nft.coinbase.com/collection/ethereum/0x9d90669665607f08005cae4a7098143f554c59ef)" NFT  ([Data Group](https://factory.sismo.io/groups-explorer?search=stand-with-crypto-nft-minters))
* Contributors to [Sismo Hub](https://github.com/sismo-core/sismo-hub) GitHub repo ([Data Group](https://factory.sismo.io/groups-explorer?search=sismo-hub-contributors-github))
* Gitcoin Passport Holders ([Data Group](https://factory.sismo.io/groups-explorer?search=gitcoin-passport-holders))
* [ENS DAO](https://docs.ens.domains/v/governance/) participants ([Data Group](https://factory.sismo.io/groups-explorer?search=ens-voters))
* Sismo Community Group ([Data Group](https://factory.sismo.io/groups-explorer?search=0xd630aa769278cacde879c5c0fe5d203c))

{% hint style="info" %}
Learn how to create a Data Group [here](create-your-data-group.md).
{% endhint %}

The Sismo Hub takes the information in a Data Group and publishes it in an onchain Merkle tree, which is used as a source of truth in Hydra proving schemes. Currently, all Sismo proving schemes belong to the Hydra family. Future proving schemes could use different sources of truth.

## What is a Group Generator?

A Group Generator, as used by Sismo, is essentially a tool that creates and manages Data Groups, making them easier to organize and access in a scalable way. The heart of the Group Generator is the 'generate' function, which creates a Data Group based on predefined details and settings. The function can run on different frequencies, specified by 'GenerationFrequency'. This could be once, daily, weekly, or monthly.

As a result, Data Groups can be viewed as snapshots of data at a given time. For example, a Data Group containing holders of a certain NFT collection could be updated on a daily basis so that proving schemes have access to the latest and most accurate data.

{% hint style="info" %}
Learn how to create a Group Generator [here](create-your-group-generator.md).
{% endhint %}

## What is a Data Provider?

Data Providers are tools used by Group Generators when creating Data Groups to programmatically fetch data from external APIs, such as Lens, GitHub, Snapshot, etc. This is particularly useful for those who wish to gather data without dealing with the complexity of directly interacting with APIs.

{% hint style="info" %}
Learn how to create a Data Provider [here](create-your-data-provider.md).
{% endhint %}

A Data Provider can support different types of Data Sources, such as web2 accounts (Twitter, GitHub, Telegram) or web3 accounts (Ethereum addresses, ENS, Lens handles), and offers functionalities like automatic and frequent group updates.

Once a Data Provider is built, it can be used in Sismo Hub and made available for others to use.
