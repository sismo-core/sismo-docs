---
cover: .gitbook/assets/TWITTER BANNERvert_1500x500px.jpeg
coverY: 0
---

# What is Sismo?

## Sismo's Core Mission

Sismo's core mission is to develop **an attestation protocol focused on decentralization, usability and privacy**. The Sismo Attestation Protocol allows anyone to generate attestations from their personal data. Sismo attestations are easy to integrate in web2 and web3 apps as a reputation and access control tool.

Sismo aims to contribute to the emergence of a decentralised version of a Single Sign On (SSO) systems, based on web3 login and user-generated attestations.

Using Sismo Wallet, one can generate, from their imported web3 source accounts, a wide range of attestations such as "Owns of BAYC NFT", "Voted 2 times in ENS DAO" or "Sent more than 100 transactions on Ethereum".

Sismo Attestations are to be initially recorded on EVM chains, packaged as badges by default. Sismo badges are NFTs (ERC-1155, non-transferrable by default) wrapping the data of Sismo Attestations. This enables Sismo Attestations to be natively integrated in applications already using NFTs as a reputation system component, to control access to their services or to curate an identity.

Sismo especially features ZK Attestations: they are attestions, powered by Zero Knowledge technology, that do not reveal the source of the attested data.

MAIN SCHEME

Wallet => Attestations <= packages]

## Sismo Attestation Protocol

Sismo Attestation Protocol (SAP) is the set of rules linked to the creation, update and deletion of attestations stored in the Sismo Attestations State (SAS).

### Sismo Attestations State (SAS)

The Sismo Attestations State (SAS) is the multi-chain database of all attestation collections created through Sismo Attestation Protocol (SAP).\
The protocol maintains a set of authorised attesters which are smart contracts allowed to write attestations in the SAS.&#x20;

The SAS has 2^256 attestation collections slots, divided in shards.&#x20;

Authorised attesters get each a dedicated shard in the SAS and receive write access on the underlying attestation collections.&#x20;

\=> Link to SAS

### Claiming Protocols

A claiming protocol enables users to generate proofs for a defined number of claims such as "Owner of Cryptopunk".&#x20;

Each claiming protocol has&#x20;

* The database which contains supported claims data, e.g a merkle tree of cryptopunk owners.
* A proving scheme:
  * A prover: code that lets users generate proofs from the database, e.g I own an address from the list of cryptopunk owners.
  * A verifier: code that enables anyone to check proofs and validate claims

There are currently two Claiming Protocol maintained by Sismo Genesis Team: SMCP and ZK-SMCP for (Zero Knowledge) Simple Merkle Claiming Protocol.

Both consume the same public and auditable database of claims, stored as account merkle trees (link) and maintained by Sismo Genesis Team.

SMCP is based on public merkle proofs while ZK-SMCP generate and verify merkle proofs in SNARKs, guaranteeing privacy for the user making the claim.

\=> Link to Claiming Protocols

### Attesters

An Attester is a smart contract using the verifier of a claiming protocol to attest claims made by users, backed by the proofs they generated with the prover.&#x20;

Once the claim is verified, the attester creates or renews an attestation for the user in the attestation collection associated with the claim.\
Each claim supported by an authorised attester gets attributed an attestation collection in the attester's reserved shard of the Sismo Attestation State (SAS).

A collection regroups all user attestations to the same claim.

For each of the claiming protocols maintained by Sismo (SMCP and ZK-SMCP), two attesters were deployed and authorized on our supported chains (mainnet and polygon).

### Sismo Badges

Sismo Badges are Non Transferable NFT (ERC1155) which are wrappers of attestations. They bring usability to attestations. The score of an attestation is encoded in the balance&#x20;

Each attestation created is natively packaged as a badge. Any application can create its own custom badge.

\=> Link to Badges and packages

## Sismo Genesis Team

Sismo Genesis Team is **an engineering team developing software to support Sismo's mission**.&#x20;

It has 3 main roles:

* Initiate the Sismo Protocol
* Develop and maintain new attestation protocols
* Develop tools around the Sismo Protocol

We are the core maintainers of Sismo Protocol. Its development and governance will be progressively handed to the Sismo DAO.

We will maintain and propose new Attestation Protocols to Sismo.

We will develop and improve Zikitor, our frontend, as well as libraries so Sismo attestations become easy to generate and use natively within apps.

## Sismo DAO

Sismo DAO is a **protocol DAO** launched in October 2021 to progressively oversee and manage the development and maintenance of Sismo Protocol. It started as a social DAO, gathering curated generations of members sharing similar interests about privacy, reputation, identity and zero-knowledge technologies.&#x20;

It is currently in charge of the allocation of Sismo DAO treasury and will move step by step towards an administrative role over Sismo Protocol.

{% content-ref url="governance/sismo-dao.md" %}
[sismo-dao.md](governance/sismo-dao.md)
{% endcontent-ref %}
