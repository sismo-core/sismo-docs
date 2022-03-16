# Sismo archi





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

