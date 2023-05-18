# Offchain Sample Project

## Prerequisites

* [Node.js](https://nodejs.org/en/download/) >= 18.15.0 (Latest LTS version)
* [Yarn](https://classic.yarnpkg.com/en/docs/install)

## Installation

In a first terminal:

```bash
# clone the repository
git clone https://github.com/sismo-core/sismo-connect-offchain-sample-project.git
cd sismo-connect-offchain-sample-project

# install frontend / backend dependencies
yarn
```

## Start your local application

In a new terminal:

```bash
# this will start your Next.js app
# the frontend is available on http://localhost:3000/
# it also starts a local backend
yarn dev
```

After this command, you will have your local application running on [http://localhost:3000](http://localhost:3000).

<figure><img src="../../.gitbook/assets/Capture d’écran 2023-05-18 à 09.40.46.png" alt="" width="375"><figcaption><p>Running app on <a href="http://localhost:3000">http://localhost:3000</a></p></figcaption></figure>

To try to register, you will need to add your address in the `config.ts` file:

```typescript
// config.ts

// Replace with your address to become eligible for the registration
// make sure to have this address in your Vault
export const yourAddress = "0x855193BCbdbD346B423FF830b507CBf90ecCc90B";
```

{% hint style="success" %}
Make sure to import the address you reference in the `config.ts` file into your Sismo Vault when you try out the sample project application. If it is not done yet when you get redirected, you can add your address by clicking on the purple "connect" button.
{% endhint %}

<figure><img src="../../.gitbook/assets/Screenshot 2023-05-11 at 16.57.13.png" alt=""><figcaption><p>Import the address referenced in the config.ts file</p></figcaption></figure>
