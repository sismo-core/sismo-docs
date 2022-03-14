---
cover: .gitbook/assets/TWITTER BANNERvert_1500x500px.jpeg
coverY: 0
description: TL;DR of everything you need to know about Sismo
---

# What is Sismo?

Sismo's mission is to bring a web3 attestation system focused on privacy, usability and decentralisation.&#x20;

Users can generate, from their web3 accounts, a wide range of attestations such as "Owner of BAYC NFT", "Voted 2 times in ENS DAO" or "Paid > 1 eth of fees on Ethereum". \
These attestations can be used to privately access gated services, such as a chat app reserved for BAYC NFT holders.

Sismo DAO owns and maintains SMA (Sismo Maintained Attestations), the database of all attestations issued by authorized attestation protocols.\
Sismo DAO is in charge of whitelisting and configuring Attestation Protocols, the only entities that have the write access on SMA. \[see more: more on attestation protocols]

Sismo Genesis Team developed a first Attestation Protocol, ZK-SAP, a Zero-Knowledge Attestation Protocol based on ZK-SNARKs. \[See more: ZK-SAP]\
\
It also developed a first easy-to-use attestation package: Sismo Badges.\
Badges is an NFT representation of attestations, so they attestations are easy to integrate  \
Holding BAYC ZK Badge <=> Received an attestation of the fact "Owner of BAYC NFT" through ZK-SAP Attestation Protocol.\
\[See more  on  Sismo Badges and Attestation Packages]&#x20;



Sismo Genesis Team focus on making it easy for:

* Users to generate attestations.
* Users to use these attestations in their apps.
* Apps to integrate these attestations, as access control infra or reputation tool.
* (Apps to request a new attested facts, related to their use case)
* (Teams to propose new attestation protocols, with different privacy vs trustlessness vs usability trade-offs)

It develops and maintains:

* Zikitor, a frontend that let users privately aggregate their source Accounts and generate attestations from them.
* Sismo Attestation Protocols: a series of attestation protocols that will be proposed for whitelisting to Sismo DAO.\


Zikitor is the user facing product. It is a privacy focused frontend that allows users to generate attestations (or more precisely provable claims that can then be verified by an attester). \
Zikitor features a vault where users can store encrypted their source Accounts from which they are willing to generate attestations.

\[more on Zikitor]\
\
Sismo will offer several attestation packages (VCs, Non Transferable NFT, Oauth system) so Sismo attestations are easy to integrate with a wide range of apps. Packaged attestations should be thought as reputation and access control tools.

\




### Sismo Attestation Protocols

Sismo DAO is in charge of curating the attestation protocols that are allowed to write into Sismo Attestations. Sismo DAO effectively distributes attestation id ranges to whitelisted attestation protocols.

more on attestation protocols x Sismo Attestations:\
1 attestation protocol => n attestor.\
Sismo attestations db is the global database of all attestations validated by Sismo. \
Initially Sismo Attestations live in two smart contracts: SismoAttestation.sol deployed on mainnet and SismoAttestations.sol deployed on polygon.\


Today, the only whitelisted Attestation Protocol is ZK-SAP. \[more on ZK-SAP]

### Sismo Badges and Attestation Packages

Sismo Badges is a simple package layer on top of Sismo Attestations. Sismo Badges follow the ERC1155 standard and allow for easy integrations with Sismo Attestations.&#x20;

\[More on Sismo Badges: 1. default badge 2. speak about the fact: several badges on one attestation]

### Other Attestation Packages

Sismo Badges are the first type of package for Sismo Attestations. \
Sismo also plans to release offchain packages that would allow to any web2 application to consume the attestation without the need to write it onchain.

### Zikitor

Sismo Frontend, Zikitor, is a secure, privacy-first front-end that allows anyone to generate, from their Ethereum accounts, attestations. \
Zikitor is thought to become the go-to where users can safely aggregate their sources of reputation and history to generate the attestations they need to access their services. It aims at replacing traditionnal web2 SSO.

Zikitor will in its first release allow users to generate ZK Badges (and underlying ochain attestations).

More on Zikitor: Open toward web2 integrations, SIWE integrations, etc..



### Attestations

Sismo enables users to create attestations from the historical data and reputation of their Ethereum accounts. These attestations can be stored on-chain as NFTs (Badges) and/or off-chain within web2 infrastructures.\


More on attestations (external link): Each attestation is defined by the underlying Attestation Protocol that was used, the data of the attestation, \
Sismo builds all&#x20;

### Sismo Badges

Attestations that are written on-chain are packaged as Non-transferrable NFTs. We call them Sismo Badges. As they follow the NFT standard, they are easily used as web3 access tokens

### Attestation Protocols

Sismo can feature several attestations protocols. Each attestation protocol has an underlying proving scheme. \
The first attestation protocol released by Sismo, ZK-SAP (for Zero-Knowldege Single Source Attestation protocol) enables users to create attestation from Zero-Knowedge proofs. Their badges are called ZK Badges because they&#x20;



Multiple Attestation protocols can co-live in Sismo ecosystem, the attestations are the result of a&#x20;

Sismo’s first release, the ZK-MOSA Protocol (Zero Knowledge Monosource Attestation Protocol) will allow users to create Zero-Knowledge attestations, packaged as ZK Badges (Non Transferrable NFTs). These ZK Badges are effectively attestations of facts suwh as "owns more than 10 ETH". They do not reveal the source of any fact (i.e the Ethereum Account which holds the ETH, and all its corresponding history).

ZK Badges, using widely adopted NFT standards, can be easily used as access control tokens for web3 apps.
