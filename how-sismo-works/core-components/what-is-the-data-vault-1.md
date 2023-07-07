---
description: Encrypted storage for personal data.
---

# Sismo Hub

## What is the Sismo Hub?

Proving schemes need access to data in a specific, agreed-upon format to function. The Sismo Hub is Sismo’s data infrastructure—open for anyone to use. It is the tool that standardizes raw data for the Sismo communication protocol, creating Data Groups.

Hydra proving schemes use onchain Merkle roots as sources of truth. The Sismo Hub generates Data Groups and transforms them into Merkle registry trees—publishing them onchain. Future proving schemes could utilize different sources of truth, such as public keys issued by centralized entities.

Data Groups essentially function as snapshots of a dataset at a given point in time. For example, to generate or verify the ownership proof of an NFT at a specific moment, both the prover and verifier need to align on a snapshot of NFT owners during that period.

{% hint style="info" %}
While Sismo manages the maintenance of Data Groups, anyone can create and register a Data Group via the Sismo Hub—either via a pull request or the [Sismo Factory](https://factory.sismo.io/) (a no-code interface).
{% endhint %}

In addition to Data Groups, the Sismo Hub manages the creation and maintenance of Data Providers. They allow for the programmatic updates of Data Groups, further ensuring that provers and verifiers always work with the most relevant data.
