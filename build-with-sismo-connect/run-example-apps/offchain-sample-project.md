# Offchain Boilerplate

This boilerplate can be used as a base to start coding offchain applications that use Sismo Connect. It is using Next.js for front ends and back ends.

You are invited to modify the following:

* `front/src/app/page.tsx` , the code of the front end using the Sismo Connect React Button to request ZK proofs from users.
* &#x20;`src/app/api/verify/route.ts` , the back-end code using the Sismo Connect TypeScript library to verify ZK proofs.

This boilerplate implements a simple user authentication to a server.

{% hint style="success" %}
Use the [Sismo Connect Cheatsheet](../sismo-connect-cheatsheet.md) to choose your own data requests
{% endhint %}

## Prerequisites

* [Node.js](https://nodejs.org/en/download/) >= 18.15.0 (Latest LTS version)
* [Yarn](https://classic.yarnpkg.com/en/docs/install)

## Installation

Repository: [https://github.com/sismo-core/sismo-connect-boilerplate-offchain](https://github.com/sismo-core/sismo-connect-boilerplate-offchain)

In a first terminal:

```bash
# clone the repository
git clone https://github.com/sismo-core/sismo-connect-boilerplate-offchain.git
cd sismo-connect-boilerplate-offchain

# install front-end/back-end dependencies
yarn
```

## Start your local application

In a new terminal:

```bash
# this will start your Next.js app
# the front end is available on http://localhost:3000/
# it also starts a local backend
yarn dev
```

After this command, you will have your local application running on [http://localhost:3000](http://localhost:3000).

<figure><img src="../../.gitbook/assets/Capture d’écran 2023-06-16 à 22.20.46.png" alt=""><figcaption><p>Running app on <a href="http://localhost:3000">http://localhost:3000</a></p></figcaption></figure>

{% hint style="success" %}
By default, the boilerplate lets you impersonate accounts to be eligible for the airdrop. If you want to learn more about it, you can read about the [Sismo Connect Configuration](../technical-documentation/sismo-connect-configuration.md).
{% endhint %}
