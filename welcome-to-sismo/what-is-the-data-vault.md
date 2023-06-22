---
description: Encrypted storage for personal data.
---

# What Is the Data Vault?

The Data Vault is encrypted storage for a user’s personal data. The accounts and other credentials that users store in the Data Vault are known as Data Sources. The granular pieces of data within Data Sources, representing valuable elements of a user’s digital identity, are known as Data Gems.

After aggregating their identity, users can prove ownership of Data Sources and Data Gems. The Data Vault is encrypted and only ever exists in its decrypted state on a user’s device.

{% hint style="info" %}
You can create your own Data Vault and start aggregating your identity [**here**](https://vault-beta.sismo.io/).
{% endhint %}

## Aggregate Your Identity

To store personal data, users must add Data Sources to their Vault. Data Sources range from web2 and web3 accounts to attestations and other credentials issued by trusted authorities. These sources of truth contain the granular data—conceptualized as Data Gems— that form a user’s aggregated digital identity yet individually are valuable pieces of social capital.

Within Data Sources lies provable data, yet it is not bound to any account or attestation in the Data Vault. Picture Data Gems as precious material that can be dislodged from its source and stashed in the Data Vault.

The following types of Data Sources can be added to the Data Vault:

* Web3 accounts (Ethereum addresses owned by a private key)
* Web2 accounts (GitHub, Twitter)
* Attestations issued by trusted authorities (coming soon)

<figure><img src="../.gitbook/assets/dv.png" alt=""><figcaption></figcaption></figure>

{% hint style="warning" %}
A valid Data Source can only be imported into a single Data Vault.
{% endhint %}

## Encrypted Storage

When creating a Data Vault, users sign a message to generate a seed—derived deterministically from the user’s ECDSA wallet signature. The wallet address used to create the Vault is its designated owner and first Data Source.

The seed generated during Vault creation is used to encrypt and decrypt the Data Vault, controlling access to the personal data inside. The Data Vault only ever exists in its decrypted state in a user’s browser—remaining fully encrypted in the Data Vault’s back end.

As the Data Vault seed is deterministic, it ensures a user can always access their Data Vault with the same ECDSA wallet signature. If a user loses access to an imported web2 account, they can regain access to their Data Vault via a generated backup key.

On the Data Vault application, users can grant access to additional wallet addresses—making them owner accounts that can decrypt and access the Vault. Linking accounts has no implications on user privacy due to the encrypted nature of the Vault.

## Data Vault in Action

Envision the Data Vault as an encrypted aggregator that securely stores various Data Sources. When accessing an application via Sismo Connect, users can choose which Data Sources and Data Gems they want to selectively disclose.

For instance, imagine a user has imported two Ethereum addresses: a public ENS domain and a private wallet holding a Gitcoin Passport. Sismo allows them to:

1. Prove ownership of their public wallet (Data Source)
2. Selectively disclose they own a Gitcoin Passport (Data Gem) without revealing their private wallet.

The Data Vault ensures users have full control over their aggregated digital identity, with the flexibility to authenticate and share information on their own terms. As the starting point for end users to leverage Sismo’s communication protocol, the Data Vault reshapes how users interact with applications and manage their digital presence.
