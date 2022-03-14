---
description: TL;DR of everything you need to know about Sismo
cover: .gitbook/assets/TWITTER BANNERvert_1500x500px.jpeg
coverY: 0
---

# Sismo Introduction

### Sismo Mission

Sismo's mission is to provide a web3 attestation system focused on privacy, usability and decentralisation. It enables anyone to generate privacy preserving attestations, usable in all their web2 and web3 applications. \
Sismo proposes to build a decentralised alternative of SSO systems, based on Sign In With Ethereum and Sismo Attestations.

Using Sismo, one can generate, from their web3 source accounts, a wide range of attestations such as "Owner of BAYC NFT", "Voted 2 times in ENS DAO" or "Paid > 1 eth of fees on Ethereum".

&#x20;When receiving a Sismo Attestation, users also receive the corresponding Sismo Badge. It is a NFT (ERC1155) package on top of the Sismo Attestation. \
\
The NFT standard being the current leading standard for access control in web3, this makes Sismo Attestations natively usable in all applications such as snapshot or guild that use the ERC1155 to gate their services.

MAIN SCHEME: \[Sources in Zikitor => Attestations => NFT Gated- services]

### Sismo First Release

Sismo first release includes:&#x20;

* Zikitor: Decentralized Identity Management Frontend
  * Users privately import, aggregate their source accounts and save them in secure vault.
  * Users can generate attestation proofs from their source accounts
* ZK-SAP: The first attestation Protocol released by Sismo
  * Allows anyone to generate ZK attestations which do not reveal anything about source accounts.
* Badges: First package on Sismo Attestation.
  * Badges, the NFT packages on top of Attestations.&#x20;
  * Since the first attestations are ZK attestations, created via ZK-SAP. We call our first badges ZK Badges.

Generally, Sismo Genesis Team main focus is on making it easy for:

* Users to generate secure attestations.
* Users to use these attestations in their apps.
* Apps to integrate these attestations, as access control or reputation tool.
* (Apps to request a new attested facts, related to their use case)
* (Teams to propose new attestation protocols, with different privacy vs trustlessness vs usability trade-offs)



