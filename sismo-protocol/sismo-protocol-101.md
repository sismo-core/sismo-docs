---
description: Understanding Sismo Protocol
---

# Sismo Protocol 101

![////////////////// ILLUSTRATION PLACEHOLDER (PROTOCOL OVERVIEW) /////////////////////////](<../.gitbook/assets/Screenshot 2022-03-07 at 16.21.38.png>)

Sismo enables anyone to:

1. Manage [source accounts](https://sismo.gitbook.io/sismo/sismo-protocol/main-concepts/sources) (Web3 accounts) where their reputation history is stored
2. Save those source accounts privately in a [shielded vault](https://sismo.gitbook.io/sismo/sismo-protocol/main-concepts/shielded-vault)
3. Mint privacy-preserving [ZK badges](https://sismo.gitbook.io/sismo/sismo-protocol/main-concepts/zk-badges) (attesting any fact imported from their sources) on any [destination account](https://sismo.gitbook.io/sismo/sismo-protocol/main-concepts/destinations) they own
4. Profit! (= flex them or use them to access fated services, exclusive airdrops, additional governance rights, etc..)

## Why Sismo?

> “There is, simply, no way, to ignore privacy. Because a citizenry’s freedoms are interdependent, to surrender your own privacy is really to surrender everyone’s.
>
> &#x20;                                                                                                                             _Edward Snowden_

### Most people don't really own their identity & reputation

You don’t own your identity on the most popular platforms of the Web2-era such ass Twitter, Facebook, or even your bank account. Instead, it is siloed and monetized by third parties. Your reputation is trapped and cannot move with you except by sharing via centralized third parties.

Web3 changes this. Your identity now lives on chain and your private keys prove that you are who you say you are. However, there are drawbacks. Your Ethereum address has a public transaction history that can reveal sensitive information. If you sign into an app with a Web3 wallet like MetaMask, you are also sharing all of this information.

{% hint style="info" %}
New users typically start off with one or two Ethereum addresses that they use for everything. It’s convenient, but those addresses quickly accumulate a revealing history of their financial data, usage, and relationships. A simple transaction sending tokens from one wallet to another is enough to link them permanently.
{% endhint %}

Over time, users develop a need for multiple disconnected accounts. This has been accelerated by the rise of NFTs (including ENS domains) as users want publicly identifiable accounts (e.g. [vitalik.eth](https://etherscan.io/address/0xd8da6bf26964af9d7eed9e03e53415d37aa96045)) while still preserving the privacy of some of their on-chain activity. The tradeoff is that this user can no longer leverage the reputation of their other accounts.

### Zero Knowledge attestations to the rescue

Sismo protocol fixes this by enabling **** anyone to issue ZK badges for reputation aggregation and privacy preserving access control.

ZK Badges are Zero-Knowledge attestations of facts imported from your other accounts, in the form of [Soulbound NFTs](https://vitalik.ca/general/2022/01/26/soulbound.html) (ERC-1155). For example, anyone to prove that they own a Bored Ape NFT so they can access a gated-website without having to reveal which specific NFT and which account it is stored in.&#x20;

Endless possibilities in terms of ZK badges can be explored to aggregate one's on-chain history and attest any type of on-chain activity.

## Sismo Protocol Main Stakeholders

_////////////////// ILLUSTRATION WITH ROLES AROUND SISMO //////////_

__

**ZK Badge Minter**

End users minting a ZK badge attesting a fact from one of their sources

**ZK Badge Creator**

Developers and project stakeholders creating new type of ZK badges

**ZK Badge Reader**

Applications and entities reading Zk badges held by a web3 profile

**Data Provider**

Entities providing data to be attested by Sismo protocol
