---
description: Dive in to tutorials on how to start building with Sismo.
---

# Start Building with Sismo

Sismo enables users and applications to get the most out of their data while preserving privacy.

Currently, Sismo has three core components:

* [Sismo Connect](../readme/sismo-connect.md) — a single sign-on method (SSO) enabling private user authentication and sovereign data verification for apps.
* [ZK Badges](../what-is-sismo/sismo-badges.md) — non-transferable tokens (SBTs) that represent verifiable statements authenticated by an attestation protocol.
* [Data Vault](../what-is-sismo/data-vault.md) — encrypted storage where users aggregate their personal data and generate zero-knowledge proofs.

## Create Data Groups, Sismo Connect apps, and ZK Badges

Behind Sismo's three core components lie the [Data Groups](../technical-concepts/data-groups.md) fundamental to Sismo. To create Sismo Connect applications or ZK Badges, builders must first create or reuse Data Groups. These Data Groups are represented as groups of Data Shards—atomic pieces of data in a user’s Data Vault that categorize users by similar characteristics.

There are currently two ways of creating Data Groups:

* Through the [Sismo Factory](https://factory.sismo.io/) — a no-code interface
* Through a pull request on the [Sismo Hub](../technical-documentation/sismo-hub/) — our open source back end

{% hint style="info" %}
By creating a Data Group, builders can subsequently create their own Sismo Connect apps or ZK Badges.&#x20;
{% endhint %}

See the following sections for full tutorials on how to use the Sismo Factory and Sismo Hub:

{% content-ref url="sismo-factory/" %}
[sismo-factory](sismo-factory/)
{% endcontent-ref %}

{% content-ref url="sismo-hub/" %}
[sismo-hub](sismo-hub/)
{% endcontent-ref %}

## Integrate Sismo Connect into your app

After getting familiar with the Sismo Factory or Sismo Hub, builders can integrate Sismo Connect into their own applications. Once integrated, applications can request private, granular data from users, while users can authenticate and selectively reveal their data.

See the following section for tutorials on how to build with sismoConnect:

{% content-ref url="sismo-connect/" %}
[sismo-connect](sismo-connect/)
{% endcontent-ref %}

