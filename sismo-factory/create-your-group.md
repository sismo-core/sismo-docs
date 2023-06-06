---
description: Non-developer tutorial
---

# Create a Data Group

This tutorial will show you how to create a [Data Group](../knowledge-base/resources/technical-concepts/data-gems-and-data-groups.md) from start to finish using the [Sismo Factory](https://factory.sismo.io/groups-explorer).

At the end of this tutorial, you will have a Group of Twitter, GitHub, and Ethereum accounts. After a short verification process (usually under 1 hour), this Data Group will be deployed on our chain(s) of choice and be available for a [Sismo Connect app](../welcome-to-sismo/what-is-sismo-connect.md) 🙌

{% hint style="info" %}
If you are a developer and want to build your own customizable Data Group, go check out this [tutorial](../knowledge-base/resources/sismo-hub/sismo-hub/create-your-group.md). It will also explain all the steps of the creation process.
{% endhint %}

{% hint style="success" %}
Don't hesitate to join our [**Dev** **Telegram**](https://t.me/+Z-SwcvXZFRVhZTQ0) and ping us during a hackathon. We would love to talk to you and meet you there. **All new Groups under 100k accounts are automatically deployed**.
{% endhint %}

## What are Data Groups?

[Data Groups](../knowledge-base/resources/technical-concepts/data-gems-and-data-groups.md) are derived from Data Sources:

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

## Data Group Use-Cases

A Data Group can be used for a [Sismo Connect app](../welcome-to-sismo/what-is-sismo-connect.md)

Let's take the [proof-of-humanity](https://github.com/sismo-core/sismo-hub/tree/main/group-generators/generators/proof-of-humanity) Group as an example and look at these use cases:

#### Sismo Connect app

Here is an instance of a Sismo Connect App that has been implemented using the proof-of-humanity Group:

<figure><img src="../.gitbook/assets/Capture d’écran 2023-05-12 à 00.19.13.png" alt=""><figcaption><p>Sismo Connect App example</p></figcaption></figure>

This Sismo Connect App allows you to gate contents/features of your app to Proof of Humanity registrants without revealing the registered addresses.

After this tutorial, you will be able to use your Group for a [Sismo Connect app](../welcome-to-sismo/what-is-sismo-connect.md).

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

<figure><img src="../.gitbook/assets/Capture d’écran 2023-03-13 à 17.32.56.png" alt=""><figcaption><p>Factory - Group page</p></figcaption></figure>

### Create the Group

Let's build our Group now 🧑‍💻

#### Title

First, you need to define a name for your Group: in our case, we will take the name "sismo-supporters"

<figure><img src="../.gitbook/assets/Capture d’écran 2023-03-13 à 17.44.42.png" alt=""><figcaption><p>Group title</p></figcaption></figure>

#### List of the Data Sources

After doing so, it is possible to add accounts using three different methods:

<figure><img src="../.gitbook/assets/spaces_DbBfd4ahlvVGlRudj8NR_uploads_pb1TyMd9h2Ok6WFkTsTB_Capture d’écran 2023-03-13 à 18.webp" alt=""><figcaption><p>Data sources fetching methods</p></figcaption></figure>

There are 3 different ways to define the data of a Group Generator, you can use:

* A defined list of Data Source ("By uploading a list of accounts")
* [Data Providers](../knowledge-base/resources/sismo-hub/data-providers.md) : function that allows fetching Data Sources ("From data providers")
* Already existing Groups ("From data group")

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

<figure><img src="../.gitbook/assets/Capture d’écran 2023-03-13 à 20.07.59.png" alt=""><figcaption><p>List of Data Source</p></figcaption></figure>

Here we can see that the 7 accounts have been found (<mark style="color:blue;">blue box</mark>). Then click on the <mark style="background-color:blue;">**Add**</mark> button (<mark style="color:red;">red box</mark>) in order to add these accounts to the group.

{% hint style="info" %}
If you need to import a huge number of Data Sources, you can also directly import them through a json file by clicking on <mark style="background-color:blue;">**Replace json**</mark> button (<mark style="color:green;">green box</mark>)
{% endhint %}

For our case, we didn't specify the value of each account, so they will all have a value of 1. But you can define the value you want for each account if desired.

For example, different values can be used for voting power:&#x20;

* "1" --> gives you 1 voting power
* "2" --> gives you 50 voting power
* "3" --> gives you 500 voting power



* **Get the list of** [**sismo.lens**](https://lenster.xyz/u/sismo) **Lens followers**

To do this, you need to use a [Data Provider,](../knowledge-base/resources/sismo-hub/data-providers.md) so we select the "_From Data Providers_" section.\
Then we choose the Lens Provider and use the `getFollowers()` function:

<figure><img src="../.gitbook/assets/Capture d’écran 2023-03-13 à 18.41.53.png" alt=""><figcaption><p>Lens Provider</p></figcaption></figure>

You need to enter the Lens profile we want to fetch the followers from (sismo.lens) as argument.

Next, by clicking on the <mark style="background-color:blue;">**Add**</mark> button (<mark style="color:red;">red box</mark>), you will know if the Lens profile exists and how many followers they have.

<figure><img src="../.gitbook/assets/Capture d’écran 2023-03-13 à 19.27.15 2.png" alt=""><figcaption></figcaption></figure>

After this, you can click on the second <mark style="background-color:blue;">**Add**</mark> button (<mark style="color:red;">red box</mark>) in order to add these Lens accounts (sismo.lens followers) to the group.

{% hint style="info" %}
If you are a developer and want to build your own Data Provider, go check out this [tutorial](../knowledge-base/resources/sismo-hub/sismo-hub/create-your-data-provider.md). It will explain all the steps of the creation process.
{% endhint %}

* **Get the holders of** [**Sismo - EthCC 2022 POAP**](https://poap.gallery/r/event/53325)

To get the holders of this POAP, we will use the Poap Provider:

<figure><img src="../.gitbook/assets/Capture d’écran 2023-03-13 à 19.33.51.png" alt=""><figcaption></figcaption></figure>

With the same logic as before, by clicking on the <mark style="background-color:blue;">**Add**</mark> button, we will add the holders of the POAP to the list of Data Sources.

* **Get the** [**Sismo Contributor Level 3**](https://github.com/sismo-core/sismo-hub/tree/main/group-generators/generators/sismo-contributors-tier3-builders)

To do this, you have to go to the "_From data group_" section and search for the group you want to use, for our case, we want to get the [sismo-contributors-tier3-builders](https://github.com/sismo-core/sismo-hub/tree/main/group-generators/generators/sismo-contributors-tier3-builders):

<figure><img src="../.gitbook/assets/Capture d’écran 2023-03-13 à 19.45.17.png" alt=""><figcaption><p>Import a group</p></figcaption></figure>

So you can select it and click on the <mark style="background-color:blue;">**Add**</mark> button.

Finally, that is what you should get at the end:

<figure><img src="../.gitbook/assets/spaces_DbBfd4ahlvVGlRudj8NR_uploads_MwhLICkywLEYoDWAHMJh_Capture d’écran 2023-03-14 à 10.webp" alt=""><figcaption><p>Final Group</p></figcaption></figure>

We will have a total of **15750** **accounts** in our group (<mark style="color:blue;">blue box</mark>), however, this number will have increased since the creation of this tutorial.

#### Update frequency

After finalizing our list of Data Source, we can select how often this group is updated. For our Sismo Supporter Group, we select ‘Weekly’ from the drop-down menu. This means that users who become a Sismo Contributor level 3 or follow Sismo on Lens after our Group is created can still become eligible after the weekly update.

{% hint style="success" %}
Don't hesitate to join our [**Dev** **Telegram**](https://t.me/+Z-SwcvXZFRVhZTQ0) and ping us during a hackathon to ask for a custom Data Group update.
{% endhint %}

<figure><img src="../.gitbook/assets/Capture d’écran 2023-03-13 à 20.26.17.png" alt=""><figcaption><p>Update frequency</p></figcaption></figure>

#### Description

Next, you will have to add 2 descriptions:

* a **one-line description** that explains briefly what your Group is composed of
* a **technical description** that lists the different requirements for a data source to be part of the Group

<figure><img src="../.gitbook/assets/Capture d’écran 2023-03-14 à 10.07.25.png" alt=""><figcaption><p>Descriptions</p></figcaption></figure>

### Deploy your Group

{% hint style="warning" %}
Before deploying your group, please be sure to:

* Have texts written in the English language
* Not have obscene content
* Not impersonate others or have otherwise malicious intent
{% endhint %}

After having completed the captcha, click on the "Request to deploy":

<figure><img src="../.gitbook/assets/Capture d’écran 2023-03-14 à 10.11.30.png" alt=""><figcaption><p>Deploy the group</p></figcaption></figure>

You will be redirected to the [Group Explorer Page](https://factory.sismo.io/groups-explorer):

<figure><img src="../.gitbook/assets/Capture d’écran 2023-03-14 à 10.11.48 2.png" alt=""><figcaption><p>wainting for approval</p></figcaption></figure>

We can see that now your Group request is in <mark style="background-color:blue;">**waiting for approval**</mark> (<mark style="color:red;">red box</mark>) and a few time after in   <mark style="background-color:blue;">**Deploying**</mark>.

Once you submit your Group, it will be **automatically accepted and deployed** on all [supported chains](../knowledge-base/resources/sismo-101.md) in \~20 minutes if it contains less than 100k accounts in it.

{% hint style="success" %}
Don't hesitate to join our [**Dev** **Telegram**](https://t.me/+Z-SwcvXZFRVhZTQ0) and ping us during a hackathon to ask for help or for a custom group update. We would love to talk to you and meet you there.
{% endhint %}

{% hint style="info" %}
If you Group consist of more than 100k accounts then the Sismo Team will have to approve the Group before deploying it. So don't hesitate to send your PR there as well, we will quickly review and merge it (**usually under 30 minutes**). During hackathons, we are committed to quickly reviewing any new Data Group pull request.
{% endhint %}

So a few time after your Group will be in deployment and it will be displayed as <mark style="background-color:green;">✔</mark> <mark style="background-color:green;"></mark><mark style="background-color:green;">**Deployed**</mark>.

You have finally created your first Group through the [Factory](https://factory.sismo.io/), congrats! 🎉

You can now use it to create a [Sismo Connect app](../welcome-to-sismo/what-is-sismo-connect.md) via the Factory.

{% hint style="info" %}
You can also click on the 🔽 icon in order to display more information on your group, including the GitHub Pull Request associated with your Group creation.

For this tutorial, you can find the generated PR [here](https://github.com/sismo-core/sismo-hub/pull/1512).
{% endhint %}

If you have any questions or you need help regarding your Group creation process, do not hesitate to join [Sismo Discord](https://discord.gg/sismo) and ask us in the **#general-support**. We will be glad to answer you 🤗
