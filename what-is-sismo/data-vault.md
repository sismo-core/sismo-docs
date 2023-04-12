---
description: Aggregate your identity.
---

# Data Vault

Sismo’s Data Vault stores a user’s encrypted personal data irrespective of its origin. The atomic data accumulated in the Vault, known as Data Gems, categorizes users into specific groups—such as being long-term Ethereum users, holders of a certain NFT, or contributors to a DAO. Data Gems are valuable pieces of social capital extracted from Data Sources—accounts (web2 or web3) or other credentials added to a user’s Data Vault.

After aggregating data inside the Vault, users can make verifiable claims about Data Gems that they own. These verifiable claims can be proven in the Vault, selectively revealed to applications (on-chain or off-chain), and verified via zkConnect or tokenized as Badges. The Data Vault enables users to verify ownership of valuable personal data (e.g, being part of the Proof of Humanity registry) and reveal it in a granular and privacy-preserving manner (e.g, proving to own more than 100 ETH held on multiple unlinked accounts without revealing them).

{% hint style="info" %}
Users can create their own Data Vault and start aggregating their identity [here](https://vault-beta.sismo.io/).
{% endhint %}

## Extract Data Gems from Data Sources

To store personal data, users must add Data Sources to their Vault. Data Sources are sources of truth that range from web2 and web3 accounts to attestations and other credentials issued by relevant authorities. These sources of truth contain the atomic data—conceptualized as Gems— that form a user’s aggregated digital identity yet individually are valuable pieces of social capital.

Within Data Sources lies provable data, yet it is not bound to any account or attestation in the Data Vault. Picture Data Gems as precious material (i.e, personal data) that can be dislodged from its source and stashed away in the Data Vault.

The following types of Data Sources can be added to the Data Vault:

* Web3 accounts (Ethereum addresses owned by a private key)
* Web2 accounts (GitHub, Twitter)
* Attestations issued by governments, institutions, etc (coming soon)

{% hint style="warning" %}
A valid Data Source can only be imported into a single Data Vault.
{% endhint %}

Web3 accounts are added to the Data Vault by signing messages that generate a deterministic seed used for decryption. This seed is also used to derive secrets for Sismo’s [Commitment Mapper](../technical-concepts/commitment-mapper.md). Web2 accounts are added via an OAuth authorization process that associates the account with a seed generated from a private key in the Data Vault.

Web3 accounts added to the Vault are designated as owners by default, though this can be modified in the application’s settings.

## Verify and reveal Data Gems

Where password managers store passwords, the Data Vault stores Data Sources and extracts the atomic data (i.e, Data Gems) about a user inside. Each Data Gem has an owner, value, and categorizes a user into a specific Data Group. Users formulate verifiable claims about Data Gems that they own to access applications via zkConnect or mint Sismo Badges.

As verifiable claims can be leveraged independently of their source, they can be seen as fungible and account-agnostic within the confines of the Data Vault. Data Gems, once extracted from the Data Sources hosting them, can be revealed without exposing the source of truth or associated owner via verifiable claims.

Inside the Data Vault, users can participate in proving schemes to establish that they own a given Data Gem. Thanks to zkConnect, these verifiable statements can be revealed to applications (either on-chain or off-chain) and verified in a trustless and privacy-preserving manner. Alternatively, verifiable statements, once verified, can be tokenized as Badges—facilitating the private transfer of data from a Data Source to a web3 address.

When adding Data Sources to the Vault and revealing the Data Gems within, users sign a message to generate an off-chain commitment. After submitting the commitment to the Commitment Mapper, the Data Vault receives a commitment receipt. This receipt verifies proof of ownership and privately associates a Data Source with a commitment. The commitment receipt is subsequently used to verify ownership in a zk-SNARK.

## Stash Data Gems in encrypted storage

When creating a Data Vault, users sign a message to generate a seed—derived deterministically from the user’s ECDSA wallet signature. The wallet address used to create the Vault is its designated owner and first Data Source. Adding a Data Source links it to the user’s Vault by creating a unique AnonUserID. Once a Data Source has been privately associated with a user’s Data Vault via an AnonUserID, it’s impossible to add the Data Source to Vaults owned by other addresses.

The seed generated during Vault creation is used to encrypt and decrypt the Data Vault, controlling access to personal data (i.e, Data Gems) inside. The Data Vault only ever exists in its decrypted state in a user’s browser—remaining fully encrypted in the Data Vault’s back end.

As the Data Vault seed is deterministic, it ensures a user can always access their Data Vault with the same ECDSA wallet signature. If a user loses access to an imported web2 account, they can regain access to their Data Vault via a generated backup key.

On the Data Vault application, users can grant access to additional wallet addresses—making them owner accounts that can decrypt and access the Vault. Linking accounts has no implications on user privacy due to the encrypted nature of the Vault.

\
\
