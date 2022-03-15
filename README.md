---
description: TL;DR of everything you need to know about Sismo
cover: .gitbook/assets/TWITTER BANNERvert_1500x500px.jpeg
coverY: 0
---

# Sismo Introduction

### Sismo Mission

Sismo's mission is to provide a web3 attestation system focused on privacy, usability and decentralisation. Sismo should allow anyone to generate privacy preserving attestations, usable in their web2 and web3 applications.\
Sismo helps to build a decentralised alternative of SSO systems, based on web3 accounts and attestations.

Using Sismo, one can generate, from their web3 source accounts, a wide range of attestations such as "Owner of BAYC NFT", "Voted 2 times in ENS DAO" or "Paid > 1 eth of fees on Ethereum".

When receiving a Sismo Attestation, users also receive the corresponding Sismo Badge. It is a NFT (ERC1155) package on top Sismo Attestations. \
\
The NFT standard being the current leading standard for access control in web3, this makes Sismo Attestations natively usable in all applications such as snapshot or guild that use the ERC1155 to gate their services.

MAIN SCHEME: \[Sources in Zikitor => Attestations => NFT Gated- services]



### Sismo Architecture

#### Zikitor: Decentralized Identity Management Front-end for Attestations

Sismo develops Zikitor, a secure Decentralized Identity Management Front-end, that let users self generate all the cryptographic primitives needed in order to request and generate attestations of different kinds.

* Users can privately import, aggregate their source accounts and save them in an encrypted vault from Zikitor.&#x20;
* Users can generate from their source accounts the cryptographic proofs needed to receive attestations

#### Authorized Attestation Protocols

From Zikitor, one can generate different kinds of attestations. Multiple attestation protocols can be supported by Sismo.&#x20;

Attestation protocols all share the same components:

* A proving scheme
  * A Prover: Used in Zikitor, allows user to generate attestation proofs that will be verified in order to receive an attestation
  * A Verifier: Onchain contract that verifies the attestations proofs
* Several attesters
  * Authorized code that can write attestations.
  * For one attestation protocol, you can have several attesters
    * One on each EVM chain
    * One offchain

Sismo DAO is in charge of authorizing new Attestation Protocols and configuring them.

Sismo DAO maintains Sismo DAO Attestations, the multichain database of all attestations create through Sismo DAO approved Attestation Protocols



Sismo DAO owns and maintains SMA (Sismo Maintained Attestations), the database of all attestations issued by authorized attestation protocols.\
Sismo DAO is in charge of&#x20;

### Sismo First Release

Sismo first release includes:&#x20;

* Zikitor: Decentralized Identity Management Frontend
  *
* ZK-SAP: The first Attestation Protocol released by Sismo
  * Allows anyone to generate ZK attestations which do not reveal anything about source accounts.
  * Zikitor enables users to generate ZK Proofs from their source accounts via ZK-SAP prover.
  * Sismo Attestations smart contracts can validate attestation requests with ZK-SAP verifier, creating&#x20;
* Badges: First package on Sismo Attestation.
  * Badges, the NFT packages on top of Attestations.&#x20;
  * Since the first attestations are ZK attestations, created via ZK-SAP. We call our first badges ZK Badges.

Generally, Sismo Genesis Team main focus is on making it easy for:

* Users to generate secure attestations.
* Users to use these attestations in their apps.
* Apps to integrate these attestations, as access control or reputation tool.
* (Apps to request a new attested facts, related to their use case)
* (Teams to propose new attestation protocols, with different privacy vs trustlessness vs usability trade-offs)



