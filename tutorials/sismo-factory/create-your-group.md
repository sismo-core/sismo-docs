---
description: Non-developer tutorial
---

# Create your Group

This tutorial will show you how to create a [Group](../../technical-documentation/zk-badge-protocol/groups.md) from start to finish using the [Sismo Factory](https://factory.sismo.io/groups-explorer).

At the end of this tutorial, you will have a [Group](../../technical-documentation/zk-badge-protocol/groups.md) of Twitter, GitHub, and Ethereum accounts. After a short verification process, this Group will be deployed on our chain(s) of choice and be available in the Factory to create a [ZK Badge](../../technical-documentation/sismo-api/badge/) or a [sismoConnect app](../../what-is-sismo/sismoconnect.md) 🙌

## What are [Groups](../../technical-documentation/zk-badge-protocol/groups.md)?

[Groups](../../technical-documentation/zk-badge-protocol/groups.md) are composed of Data Sources:

* Web2 Data Sources: Twitter, GitHub
* Web3 Data Sources: Ethereum addresses, ENS, Lens handles

Here's an example of what a Group looks like:

```json
"0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045": "1",
"dhadrien.sismo.eth": "3",
"bigq11.eth": "10",
"martingbz.lens":"2",
...
```

If you want more info on Groups, check out this [page](../../technical-documentation/zk-badge-protocol/groups.md).

## Group use-cases

For now, you can create 2 different usages on top of Groups:

* [ZK Badges](../../technical-documentation/zk-badge-protocol/badges.md)
* [sismoConnect app](../../what-is-sismo/sismoconnect.md)

Let's take the [proof-of-humanity](https://github.com/sismo-core/sismo-hub/tree/main/group-generators/generators/proof-of-humanity) Group as an example and look at these use cases:

#### [ZK Badge](../../technical-documentation/zk-badge-protocol/badges.md)

Here you can see an [example](https://app.sismo.io/?badge=proof-of-humanity-zk-badge) of a Badge created from a Group:

<figure><img src="../../.gitbook/assets/Capture d’écran 2023-03-07 à 16.18.19.png" alt=""><figcaption><p>Proof of Humanity ZK Badge</p></figcaption></figure>

This Badge was generated from the [proof-of-humanity](https://github.com/sismo-core/sismo-hub/tree/main/group-generators/generators/proof-of-humanity) Group. The Sismo tech stack ensures that only Proof of Humanity registered addresses can mint the Badge (on the address of their choice) without revealing the registered address.

#### sismoConnect app

Here is an instance of a sismoConnect App that has been implemented using the proof-of-humanity Group:

<figure><img src="../../.gitbook/assets/Capture d’écran 2023-03-10 à 14.06.38 2 copie.png" alt=""><figcaption><p>sismoConnect App example</p></figcaption></figure>

This sismoConnect App allows you to gate contents/features of your app to Proof of Humanity registrants without revealing the registered addresses.&#x20;

After this tutorial, you will be able to create a [ZK Badge](../../technical-documentation/sismo-api/badge/) or a [sismoConnect app](../../what-is-sismo/sismoconnect.md) from your Group through the [Factory](https://factory.sismo.io/).

## Tutorial use case

In this tutorial, we will create a group composed of:

* Sismo Lens Followers 🌿
* Sismo EthCC Booth attendees 💜
* Sismo contributor Level 3 🔨
* Some Sismo core team members 🎭

## Creation of the Group

### Group creation page

First, go to the Factory (in Data Group section): [https://factory.sismo.io/groups-explorer](https://factory.sismo.io/groups-explorer)

Next, you will have to sign in to the factory with your Ethereum address. To do this click on the login button at the top left corner and sign the message.

Once you are logged in, click on <mark style="background-color:blue;">**Create new Data Group**</mark> (<mark style="color:red;">red box</mark>):

<figure><img src="../../.gitbook/assets/Capture d’écran 2023-03-13 à 17.32.56.png" alt=""><figcaption><p>Factory - Group page</p></figcaption></figure>

### Create the Group

Let's build our Group now 🧑‍💻

#### Title

First, you need to define a name for your Group: in our case, we will take the name "sismo-supporters"

<figure><img src="../../.gitbook/assets/Capture d’écran 2023-03-13 à 17.44.42.png" alt=""><figcaption><p>Group title</p></figcaption></figure>

#### List of the Data Sources

After doing so, it is possible to add accounts using three different methods:

<figure><img src="../../.gitbook/assets/spaces_DbBfd4ahlvVGlRudj8NR_uploads_pb1TyMd9h2Ok6WFkTsTB_Capture d’écran 2023-03-13 à 18.webp" alt=""><figcaption><p>Data sources fetching methods</p></figcaption></figure>

There are 3 different ways to define the data of a Group Generator, you can use:

* A defined list of Data Source ("By uploading a list of accounts")
* [Data Providers](../../technical-documentation/sismo-hub/data-providers.md) : function that allows fetching Data Sources ("From data providers")
* Already existing [Groups](../../technical-documentation/zk-badge-protocol/groups.md) ("From data group")



**For our case we need 3 main things:**

* **Define the Sismo core team members**

To do this, go to the "_By uploading a list of accounts_" section, and put your list of Data Source.

Here are the different formats of Data Sources you can use:

* Ethereum addresses (e.g. `"0x123...456":"1"`)
* ENS (e.g. `"sismo.eth":"1"`)
* Lens handles (e.g. `"sismo.lens":"1"`)
* GitHub accounts (e.g. `"github:leosayous21":"1"`)
* Twitter accounts (e.g. `"twitter:leosayous21":"1"`

Here we will add some of the Sismo core team members:

<figure><img src="../../.gitbook/assets/Capture d’écran 2023-03-13 à 20.07.59.png" alt=""><figcaption><p>List of Data Source</p></figcaption></figure>

Here we can see that the 7 accounts have been found (<mark style="color:blue;">blue box</mark>). Then click on the <mark style="background-color:blue;">**Add**</mark> button (<mark style="color:red;">red box</mark>) in order to add these accounts to the group.

{% hint style="info" %}
If you need to import a huge number of Data Sources, you can also directly import them through a json file by clicking on <mark style="background-color:blue;">**Replace json**</mark> button (<mark style="color:green;">green box</mark>)
{% endhint %}

For our case, we didn't specify the value of each account, so they will all have a value of 1. But you can define the value you want for each account if desired.

For example: at Sismo, we use a the Sismo Contributor ZK Badge to vote on Sismo DAO Proposals. There are 3 different values in the Group:

* "1" --> gives you 1 voting power
* "2" --> gives you 50 voting power
* "3" --> gives you 500 voting power



* **Get the list of** [**sismo.lens**](https://lenster.xyz/u/sismo) **Lens followers**

To do this, you need to use a Data Provider, so we select the "_From Data Providers_" section.\
Then we choose the Lens Provider and use the `getFollowers()` function:

<figure><img src="../../.gitbook/assets/Capture d’écran 2023-03-13 à 18.41.53.png" alt=""><figcaption><p>Lens Provider</p></figcaption></figure>

You need to enter the Lens profile we want to fetch the followers from (sismo.lens) as argument.

Next, by clicking on the <mark style="background-color:blue;">**Add**</mark> button (<mark style="color:red;">red box</mark>), you will know if the Lens profile exists and how many followers they have.

<figure><img src="../../.gitbook/assets/Capture d’écran 2023-03-13 à 19.27.15 2.png" alt=""><figcaption></figcaption></figure>

After this, you can click on the second <mark style="background-color:blue;">**Add**</mark> button (<mark style="color:red;">red box</mark>) in order to add these Lens accounts (sismo.lens followers) to the group.

* **Get the holders of** [**Sismo - EthCC 2022 POAP**](https://poap.gallery/r/event/53325)****

To get the holders of this POAP, we will use the Poap Provider:

<figure><img src="../../.gitbook/assets/Capture d’écran 2023-03-13 à 19.33.51.png" alt=""><figcaption></figcaption></figure>

With the same logic as before, by clicking on the <mark style="background-color:blue;">**Add**</mark> button, we will add the holders of the POAP to the list of Data Sources.

* **Get the** [**Sismo Contributor Level 3**](https://github.com/sismo-core/sismo-hub/tree/main/group-generators/generators/sismo-contributors-tier3-builders)****

To do this, you have to go to the "_From data group_" section and search for the group you want to use, for our case we want to get the [sismo-contributors-tier3-builders](https://github.com/sismo-core/sismo-hub/tree/main/group-generators/generators/sismo-contributors-tier3-builders):

<figure><img src="../../.gitbook/assets/Capture d’écran 2023-03-13 à 19.45.17.png" alt=""><figcaption><p>Import a group</p></figcaption></figure>

So you can select it and click on the <mark style="background-color:blue;">**Add**</mark> button.

Finally, that is what you should get at the end:

<figure><img src="../../.gitbook/assets/spaces_DbBfd4ahlvVGlRudj8NR_uploads_MwhLICkywLEYoDWAHMJh_Capture d’écran 2023-03-14 à 10.webp" alt=""><figcaption><p>Final Group</p></figcaption></figure>

We will have a total of **15750** **accounts** in our group (<mark style="color:blue;">blue box</mark>), however, this number will have increased since the creation of this tutorial.

#### Update frequency

After finalizing our list of Data Source, we can select how often this group is updated. For our Sismo Supporter Group, we select ‘Weekly’ from the drop-down menu. This means that users who become a Sismo Contributor level 3 or follow Sismo on Lens after our Group is created can still become eligible after the weekly update.

<figure><img src="../../.gitbook/assets/Capture d’écran 2023-03-13 à 20.26.17.png" alt=""><figcaption><p>Update frequency</p></figcaption></figure>

#### Description

Next, you will have to add 2 descriptions:

* a **one-line description** that explains briefly what your Group is composed of
* a **technical description** that lists the different requirements for a data source to be part of the Group

<figure><img src="../../.gitbook/assets/Capture d’écran 2023-03-14 à 10.07.25.png" alt=""><figcaption><p>Descriptions</p></figcaption></figure>

### Deploy your Group

After having completed the captcha, click on the "Request to deploy":

<figure><img src="../../.gitbook/assets/Capture d’écran 2023-03-14 à 10.11.30.png" alt=""><figcaption><p>Deploy the group</p></figcaption></figure>

You  will be redirected to the [Group Explorer Page](https://factory.sismo.io/groups-explorer):

<figure><img src="../../.gitbook/assets/Capture d’écran 2023-03-14 à 10.11.48 2.png" alt=""><figcaption><p>wainting for approval</p></figcaption></figure>

We can see that now your Group request is in <mark style="background-color:blue;">**waiting for approval**</mark> (<mark style="color:red;">red box</mark>).

### Verification Process

Once we submitted our pull request, it will be checked by the Sismo core team within 48 hours.&#x20;

To get verified, your Group must:

* Be written in the English language
* Not contain obscene content
* Not impersonate others or have otherwise malicious intent

{% hint style="info" %}
Read the full guidelines here: [https://sismo.notion.site/ZK-Badge-Creation-Guidelines-ee21ee33203b4d92bcd4668df311d67c](https://sismo.notion.site/ZK-Badge-Creation-Guidelines-ee21ee33203b4d92bcd4668df311d67c)
{% endhint %}

So at this point, you have to wait for a Sismo Core team member to accept your group, which will be done in less than 48 hours.

{% hint style="info" %}
You can also click on the 🔽 icon in order to display more information on your group, including the GitHub Pull Request associated with your Group creation.

For this tutorial, you can find the generated PR [here](https://github.com/sismo-core/sismo-hub/pull/1512).
{% endhint %}

Once your Group has been verified by the Sismo core team, it will be displayed as <mark style="background-color:blue;">**Deploying**</mark>.

When your Badge has been deployed on-chain, it will be displayed as <mark style="background-color:green;">**Deployed**</mark>.

You have finally created your first Group through the [Factory](https://factory.sismo.io/), congrats! 🎉

You can now use it to create a [ZK Badge](../../technical-documentation/sismo-api/badge/) or a [sismoConnect app](../../what-is-sismo/sismoconnect.md) via the Factory.

If you have any questions or you need help regarding your Group creation process, do not hesitate to join [Sismo Discord](https://discord.gg/sismo) and ask us in **🧱︱build-on-sismo**. We will be glad to answer you 🤗
