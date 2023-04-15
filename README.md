---
description: Get the most out of your data.
cover: .gitbook/assets/Docs_1900x400.png
coverY: 7
---

# What is Sismo?

Sismo enables users to aggregate their digital identities and selectively reveal data derived from their web2 or web3 accounts.

Users aggregate their identity in Sismo’s Data Vault and start accumulating Data Gems—atomic pieces of data that categorize users into groups. In turn, users can generate proofs to make claims about their data (e.g, I own a specific NFT). These proofs are verified by applications—either on-chain or off-chain. The resulting privacy-preserving attestations—stored in on-chain smart contracts or off-chain databases—are utilized by applications for access control and reputation curation.

Standing at the crossroads between digital identity, web3 social, and zero-knowledge technology, Sismo has three core concepts:

<table data-view="cards"><thead><tr><th data-card-target data-type="content-ref"></th><th></th><th data-hidden></th><th data-hidden></th><th data-hidden data-card-cover data-type="files"></th></tr></thead><tbody><tr><td><a href="what-is-sismo/data-vault.md">data-vault.md</a></td><td>Aggregate your identity</td><td></td><td></td><td><a href=".gitbook/assets/Gitbook-Vault.png">Gitbook-Vault.png</a></td></tr><tr><td><a href="what-is-sismo/sismo-badges.md">sismo-badges.md</a></td><td>Tokenize your identity </td><td></td><td></td><td><a href=".gitbook/assets/Gitbook-Badge.png">Gitbook-Badge.png</a></td></tr><tr><td><a href="readme/sismo-connect.md">sismo-connect.md</a></td><td>The crypto-native SSO</td><td></td><td></td><td><a href=".gitbook/assets/Gitbook-ZkConnect.png">Gitbook-ZkConnect.png</a></td></tr></tbody></table>


**Anyone can start building with Sismo via the [Factory](https://factory.sismo.io/create-badge).**

## Aggregate your identity&#x20;

Accounts on both web2 and web3 have become synonymous with a user’s digital identity. Users build their identities as they accumulate Data Gems—valuable pieces of data that categorize users into groups.

While web3 puts people in control of their data, it remains segregated on isolated accounts. Sismo allows users to aggregate their identities by importing owned accounts into Sismo’s Data Vault—whether web2 or web3. The Vault is the encrypted hub that facilitates the transfer of social capital from imported accounts to applications without sacrificing privacy.

In this sense, Sismo provides users with the tools they need to choose how they reveal their data and curate their identities without sacrificing privacy.

**Users can create their own Data Vault and start aggregating their identity [here](https://vault-beta.sismo.io/).**

## Prove and verify

On a fundamental level, Sismo allows users to participate in proving schemes that verify users own certain Data Gems and thus belong to certain groups. All users have elements of their digital identities that can be categorized into groups—such as being a long-term Ethereum user, voter in a DAO, or contributor to various projects. By participating in these schemes, users can verify valuable personal data and bring it to applications without exposing unnecessary information—such as their entire wallet history.

Sismo revolves around attestations—tokenizable as [Badges](what-is-sismo/sismo-badges.md)—that prove facts about a user’s data. Badges act as a reputation curation tool, yet are just one way of verifying data. ZK proofs—a verification method that authenticates verifiable claims without revealing _how_ they are true—are attested by verifiers on-chain or off-chain. Sismo’s Badge minting [protocol](technical-documentation/zk-badge-protocol/) leverages these verifiers to issue ERC1155 tokenized attestations known as Badges.

## Bring data to any app

Sismo is particularly useful for applications seeking to gate their services or enable reputation importation. When accessing features on apps in the web3 social space and beyond, users are increasingly required to prove their eligibility to do so. [Sismo Connect](readme/sismo-connect.md) is a single sign-on (SSO) method that allows users to bring their identities to applications in a privacy-preserving manner.

When integrated into applications, Sismo Connect can request private, granular data from a user’s Data Vault, while users can authenticate and selectively reveal their data. By clicking an integrated Sismo Connect button, users launch a frictionless experience that requires minimal input.

As Sismo Connect merely enhances web3’s existing Sign-In with Ethereum (SIWE) infrastructure, neither users nor integrators are exposed to the risks inherent in centralized web2 platforms. Sismo Connect promotes privacy and usability—allowing users to leverage their data without the implications of big tech overwatch.

**Developers can learn how to integrate Sismo Connect [here](tutorials/sismo-connect/request-data-privately-with-sismo-connect.md).**
