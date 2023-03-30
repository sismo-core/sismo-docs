# Groups

The Sismo protocol revolves around the notion of **Groups of accounts**. An account is the pair of an account identifier (e.g an Ethereum address or a Github account) and a value.

A **Group of accounts** bundles **accounts that share some reputational or historical characteristics**.&#x20;

{% hint style="info" %}
Examples of such groups include:&#x20;

* ENS DAO voters. Account values = number of votes
* Ethereum Clients GitHub Contributor. Account values = number of commits
* Twitter accounts. Account values = number of followers
* Early Ethereum users. Account values = number of transactions made before 2018
* Accounts with high DeFi reputation. Account values = DegenScore score
* Early Lens profile minters. Account values = date of minting
* Gitcoin grant donors. Account values = Number of donations
{% endhint %}

{% hint style="success" %}
Anyone can propose a new group. Learn how to make your own with our tutorials:

* Developer: [create-your-group.md](../../tutorials/sismo-hub/create-your-group.md "mention")
* Non-developer: [create-your-group.md](../../tutorials/sismo-factory/create-your-group.md "mention")
{% endhint %}

### Group membership claims

Attesters issue attestations from **claims about** **group membership** that are made by users. Groups must be available to the attester.

![Group membership claims](<../../.gitbook/assets/2\_Groups (2).png>)

Technically, a user claim consist of:

* A targeted available group identifier ("I own an account that's part of the 'ENS DAO voters' group")
* A claimed value ("My account has a value of >3; I voted more than 3 times")
* (Optional) Some arbitrary data ("The group of 'ENS DAO voters' was generated on June 12th, 2022")

### Group Generation

Sismo has built an off-chain infrastructure that periodically generates off-chain groups. Groups are bi-dimensional. These off-chain groups are reusable, they can be used to make available groups for several attesters.&#x20;

{% hint style="info" %}
The off-chain group of ENS DAO Voters can be made available to several on-chain attesters
{% endhint %}

![Group generation](<../../.gitbook/assets/2\_Groups-Lists (1).png>)

Every off-chain group has a name and a generation timestamp.

Groups can be combined by creating unions, intersections, and more complex operations. [More on groups](../../build-on-sismo/sismo-protocol-overview.md). Groups can also be hybrid and constituted of Ethereum accounts and GitHub accounts in the same group.

{% hint style="success" %}
Find the list of all group generators in the [Sismo Hub Repository](https://github.com/sismo-core/sismo-hub/tree/main/group-generators/generators).
{% endhint %}

{% hint style="info" %}
We can encode some hashed properties in the available group identifiers such as availableGroupIndex = hash(groupIndex, generationTimestamp).
{% endhint %}
