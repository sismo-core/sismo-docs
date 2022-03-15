---
description: TL;DR of everything you need to know about Sismo
cover: .gitbook/assets/TWITTER BANNERvert_1500x500px.jpeg
coverY: 0
---

# Sismo Introduction

### Sismo Core Mission

Sismo's main mission is to develop an attestation protocol focused on privacy, usability and decentralisation. The protocol should allow anyone to generate privacy preserving attestations, easily integrable in their web2 and web3 applications.\
\
Sismo aims to participate in building the better alternative of current centralised SSO systems.  The decentralized version, based on web3 accounts and attestations, should let user reveal what they want or need when connecting to applications.

Using Sismo and its frontend Zikitor, one can generate, from their web3 source accounts, a wide range of attestations such as "Owner of BAYC NFT", "Voted 2 times in ENS DAO" or "Paid > 1 eth of fees on Ethereum".

Sismo Attestations are by default packaged as Badges. It is a NFT (ERC1155) package on top Sismo Attestations. This makes Sismo Attestations natively usable in all applications that use NFTs to gate their services.

MAIN SCHEME Sismo Protocol: \[Sources in Zikitor => Attestations => NFT Gated- services]

### Sismo Protocol and the Sismo Attestation State

Sismo protocol is the set of rules linked to the creation, update and deletion of attestations in the Sismo Attestation State. The Sismo Attestations State (SAS) is the cross-chain database of all attestations created through Sismo Protocol.

Sismo DAO will govern the Sismo protocol. It will especially authorize/revoke authorized attestation protocols. Authorized attestations protocol are allowed to write in the Sismo Attestation State, each one gets allocated a slot in Sismo Attestation State.

### Sismo Genesis Team



### Sismo DAO

###

###

###

### Sismo Architecture

#### Zikitor: Decentralized Identity Management Front-end for Attestations

Sismo develops Zikitor, a secure Decentralized Identity Management Front-end, that let users self generate all the cryptographic primitives needed in order to request and generate attestations of different kinds.

* Users can privately import, aggregate their source accounts and save them in an encrypted vault from Zikitor.&#x20;
* Users can generate from their source accounts the cryptographic proofs needed to receive attestations

#### Attestation Protocols

From Zikitor, one can generate different kinds of attestations. Multiple attestation protocols can be supported by Sismo.&#x20;

Sismo Genesis Team is focused on building ZK Attestations protocols, Attestation protocols that guarantee that no information is leaked from the attestation source accounts.&#x20;

It will be possible for external teams to propose new kinds of attestation protocols, with different tradeoffs on privacy, decentralization and usability.

Attestation protocols all share the same components:

* A proving scheme
  * A Prover: Used in Zikitor, it allows user to generate attestation proofs that will be verified when claiming an attestation
  * A Verifier: Code that is able to verifies the attestations proofs
* Several attesters
  * Authorized code, validating attestation request using the verifier, that can write attestations.
  * For one attestation protocol, you can have several attesters
    * One on each EVM chain
    * One offchain

Sismo DAO will be in charge of authorizing new Attestation Protocols and configuring them. As of today, Sismo DAO is only consulted by Sismo Genesis Team.

Sismo DAO effectively maintains Sismo Maintained Attestations (SMA), the aggregated database of all attestations created through Sismo DAO authorized Attestation Protocols. \
SMA is thought to become the place to go for any project that wants to generate usable and trusted attestations.

External teams can propose to Sismo DAO to accept a new attestation protocol that when accepted will be able to write in SMA.

#### Attestations, Badges and Packages

Sismo Attestations are standardised. The format is very simple, each attestation has

* attestationId
* timestamp&#x20;
* value&#x20;
* owner

This allows multiple attestation protocols to co-live and write in the same Sismo Maintained Attestations database.

On top of Attestations, Sismo offers packages, a layer on top of attestation aimed at integrators.\
With every Sismo Attestation comes a default Sismo Badge.

It is an NFT interface on top of attestations where the validity of the attestation is encoded in the balance of the Badge.

For instance if balanceOf(Badge #3) = 0 means you do not have the attestation #3.

\[need to tell more in section more precisely]

### Sismo First Release

Sismo first release includes:&#x20;

* Zikitor: Decentralized Identity Management Frontend
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



