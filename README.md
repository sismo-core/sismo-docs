---
description: TL;DR of everything you need to know about Sismo
cover: .gitbook/assets/TWITTER BANNERvert_1500x500px.jpeg
coverY: 0
---

# Sismo 101

## Sismo's Core Mission

Sismo's core mission is to develop **a protocol overseeing an attestation management system focused on privacy, usability and decentralisation**. The protocol should allow anyone to generate attestations easily integrable in any web2 and web3 application.

Using Sismo and its frontend Zikitor, one can generate, from their web3 source accounts, a wide range of attestations (= certificates proving any state of facts) such as "Owner of BAYC NFT", "Voted 2 times in ENS DAO" or "Paid > 1 ETH of fees on Ethereum".

Sismo's goal is to enable the generation of attestations that can be used, in associating with web3 login systems, as an alternative of centralised Single Sign-On systems (SSO).\
"Sign-In With Ethereum" login associated with Sismo attestations are a minimum viable implementation of such a sovereign SSO.

Sismo Attestations are to be packaged as badges by default. Sismo badges are NFTs (ERC-1155, non-transferrable by default) wrapping the data of Sismo Attestations. This enables Sismo Attestations to be natively integrated in any application already using NFTs as a reputation system component, to control access to their services or to curate an identity.

MAIN SCHEME

\[Centré autour des Sismo attestations. Centré autour du user qu utilise zikitor pour créer son attestation. Il peut ensuite utiliser cette attestation pour SIWE/ faire qqchose onchain]

## Sismo Protocol



Sismo Protocol is the set of rules linked to the creation, update and deletion of attestations in  Sismo Attestations State. Sismo Attestations State (SAS) is the cross-chain database of all attestations created through Sismo Protocol.

Sismo Protocol will be governed by Sismo DAO whose responsibility will be, among other things, to authorize new Attestation Protocols or revoke others. Authorized Attestation Protocol are allowed to write in Sismo Attestations State and are allocated a dedicated slot (a range of attestation IDs).

![](.gitbook/assets/SAS.jpeg)

## Sismo Genesis Team

Sismo Genesis Team is **an engineering team developing software to support Sismo's mission**.&#x20;

It has 3 main roles:

* Develop Sismo Protocol core software,
* Build and maintain Zikitor Frontend to access Sismo Protocol,
* Create new ZK Attestation Protocols to be authorized by Sismo DAO.

The development and governance of the protocol will be progressively handed to the Sismo DAO.

The end goal of the Genesis Team is for the protocol to be governed autonomously and to foster the emergence of new frontends competing with Zikitor.

While Sismo Genesis Team will be mainly focused on proposing new Attestation Protocols focused on decentralized and trustless privacy, external teams will be able to create and add Attestations Protocols with different tradeoffs related to privacy, centralisation and transparency.

## Sismo DAO

Sismo DAO is a **protocol DAO** launched in October 2021 to progressively oversee and manage the development and maintenance of Sismo Protocol. It started as a social DAO, gathering curated generations of members sharing similar interests about privacy, reputation, identity and zero-knowledge technologies.&#x20;

Through progressive decentralization, it is now in charge of the allocation of a DAO treasury and will move step by step towards an administrative role over Sismo Protocol.

{% content-ref url="governance/sismo-dao.md" %}
[sismo-dao.md](governance/sismo-dao.md)
{% endcontent-ref %}
