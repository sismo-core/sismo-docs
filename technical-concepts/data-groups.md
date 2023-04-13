# Data Groups

Data Groups are fundamental to Sismo—allowing users to prove facts about their data while enabling applications to gate particular features and services. Anyone building with Sismo must utilize Data Groups to request data via Sismo Connect or create ZK Badges.

Users in the same Data Group can bring the same element of their identity to gated applications. Data Groups contain pieces of data that categorize users by similar characteristics. Users become members of Data Groups by owning Data Gems of the same category. Data Gems are obtained by importing Data Sources (web2 or web3 accounts) into a user’s Data Vault.

{% hint style="info" %}
Examples of Data Groups include:

* Contributors to The Merge on Ethereum
* Donators to Gitcoin grants
* Users on the Proof of Humanity register
* Ethereum power users
* Early Lens profile minters
* Contributors to specific GitHub repositories
{% endhint %}

## Creating Data Groups

To create a Sismo Connect application or ZK Badge, users need to create or re-use existing Data Groups. These Data Groups are used as sources of truth from which users can make verifiable claims (e.g, being part of the Proof of Humanity Data Group).

There are currently two ways to create Data Groups:

* Through Sismo’s [Factory](https://factory.sismo.io/) no-code interface
* Through a Pull Request in the [Sismo Hub](../technical-documentation/sismo-hub/) - the Factory’s open-source back end

{% hint style="success" %}
Anyone can propose a new group. Learn how to make your own with the following tutorials:

Factory: [create-your-group.md](../tutorials/sismo-factory/create-your-group.md "mention")

Sismo Hub: [create-your-group.md](../tutorials/sismo-hub/create-your-group.md "mention")
{% endhint %}

Alternatively, users can make use of Data Groups created for other ZK Badges or Sismo Connect applications. For example, [zkdrop.io](http://zkdrop.io) makes use of the [the-merge-contributor](https://github.com/sismo-core/sismo-hub/tree/main/group-generators/generators/the-merge-contributor) Data Group to gate features to The Merge contributors. This Data Group can be reused to gate features on entirely different applications to the same set of users.

## Data Group Characteristics

Data Groups are essentially collections of Data Gems that share the same characteristics. A Data Gem is a piece of data with the following:

* An owner - i.e, an associated Ethereum address or web2 account
* A value - i.e, a number corresponding to a certain level of reputation within the Data Group
* A type - i.e, a unique `groupId` combined with a timestamp (latest by default) that sorts users into a specific Data Group

{% hint style="info" %}
[Data Providers](../technical-documentation/sismo-hub/data-providers.md) allow Data Groups to be frequently updated via automated data fetching.
{% endhint %}

Therefore, a Data Group consists of Data Gems of the same type, with each individual Data Gem having a unique owner and associated value. For example, holders of NFTs in the same collection own a similar Data Gem and are thus in the same Data Group. The number of NFTs held by a particular user is represented by the Data Gem’s value.

## Verifiable Claims & Data Groups

When using Sismo Connect or minting ZK Badges, users make verifiable claims about their data that is subsequently verified by applications. By making these verifiable claims, users claim to be a part of a specific Data Group.

Technically, a verifiable claim consists of:

* A targeted `groupId` ("I own an account that's part of the 'ENS DAO voters’ group")
* A claimed value and claimed type ("My account has a value of >3; I voted more than 3 times")
* (Optional) Timestamp or other miscellaneous data ("The group of 'ENS DAO voters' was generated on June 12th, 2022")
