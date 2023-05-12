---
description: Tokenize your identity.
---

# ZK Badges

ZK Badges are non-transferable tokens (SBTs) that represent verifiable claims authenticated by Sismo’s [ZK Badge protocol](zk-badge-protocol/).

Fundamentally, Badges prove ownership of specific Data Gems—derived from Data Sources stored in a user’s Data Vault. Data Gems are valuable aspects of a user’s personal data that categorize users into specific groups—such as being long-term Ethereum users, contributors to a specific GitHub repository, or French citizens.

{% hint style="success" %}
Anyone can create their own Badge on the Sismo [Factory](https://factory.sismo.io/).
{% endhint %}

## Tokenized attestations

When minting a Badge, a user generates a proof to authenticate an anonymized verifiable claim about Data Gems that they own. This proof is then verified and tokenized as a Badge by Sismo’s attester smart contract.

Once a verifiable claim has been verified, it is recorded as an attestation by the Sismo protocol. Badges are therefore tokenized attestations issued by attesters—smart contracts that convert proven data into non-transferable tokens (SBTs). As Badges are issued as tokens on-chain, they are compatible with the burgeoning ecosystem of web3 applications.

As such, Badges represent verified acts about a user’s digital identity. For example, a user could have a ZK Badge proving they have a certain threshold of Twitter followers, hold a certain NFT, or are active contributors to a particular community.

## ZK Badges

ZK Badges are privacy-preserving Badges issued by ZK attesters such as the Hydra-S1 ZK attester. The attester awards users ZK Badges after providing the protocol with a zero-knowledge proof—a verification method that proves a claim without revealing _how_ it is true.

Zero-knowledge proofs allow users to privately leverage the social capital in their Data Vault—ensuring they can selectively reveal facts about their identities to access particular applications or services. ZK Badges ensure that the user only shares the information necessary to access a specific gated application instead of their entire web3 history.

Picture a user with two wallets—one public and one private. The user has valuable social capital on each wallet yet is uncomfortable linking the accounts. Using ZK Badges, they can selectively reveal their data to web3 applications without sacrificing privacy.

## Reputation curation

On web3, owning certain pieces of data (i.e, Data Gems) can be considered valuable social capital. Badges allow the transfer of this social capital from one account to another in a privacy-preserving and granular way. In this sense, Badges facilitate reputation curation and can enable access to web3 applications via Sign-In with Ethereum (SIWE).

Badges excel at reputation curation in particular—giving users a way to import and aggregate data from their private accounts to their public-facing accounts (such as an ENS address). Sismo’s Badge minting application can therefore be viewed as a reputation curation tool that converts verifiable claims into verified on-chain SBTs.

{% hint style="info" %}
Users can mint Badges they are eligible for on Sismo's minting [app](https://app.sismo.io/).
{% endhint %}

## Badge characteristics

As Badges are tokens adhering to the ERC1155 standard, they can be accepted by any web3 application (e.g. [Guild](https://guild.xyz/sismo) or [Snapshot](https://snapshot.org/#/sismo.eth)) for access control and reputation curation. Each Badge is a unique token within the same [ERC1155 smart contract](https://polygonscan.com/token/0xf12494e3545d49616d9dfb78e5907e9078618a34). Despite being part of the same collection, different Badges behave like distinct ERC20 tokens.

A Badge can be held by multiple users, with each user having a balance corresponding to their Badge’s level. Badge levels can translate to benefits such as increased [governance voting power](http://governance.sismo.io/) or a higher social reputation.

Anyone can make their own Badges and even build or propose new attesters that award them. Each attester has its own parameters for verifying user claims and issuing Badges—pathing the way for multiple use cases.
