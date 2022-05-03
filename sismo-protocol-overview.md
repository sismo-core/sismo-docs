---
cover: .gitbook/assets/TWITTER BANNERvert_1500x500px.jpeg
coverY: 0
---

# Sismo Protocol Overview

The Sismo Protocol is the set of rules linked to the creation, update and deletion of attestations stored in Sismo Attestations Registries (SARs).

An **attestation** is a user **claim**, issued and verified by an **attester** (smart contract).

Similarly to the NFT standard ERC1155 and its NFT collections, Attestation Registries feature **attestation collections.** A collection regroups owners of the same type of attestation.

Sismo Governance is in charge of curating attesters that are allowed to write attestations in Sismo Attestations Registries.

{% hint style="info" %}
We will illustrate this overview section with the example of a user who owns 5 BAYC NFTs on `0x1`.&#x20;

This user wants to generate an attestation of BAYC ownership, through our first authorized Attester (It is called ZK-SMPS Attester, it is an attester that verifies claims using ZK Proofs, it does not leak the source of data `0x1`).&#x20;

They want to receive the attestation on `0x2`, on Ethereum mainnet's Sismo Attestation Registry (SAR).

It means the user will&#x20;

* Make a claim (claim of BAYNC NFT Ownership)
* Generate a proof using `0x1`(via the offchain ZK Snark prover of the ZK-SMPS Proving Scheme)
* Send this claim, along its proof to the attester (smart contract which verify the claim against the ZK Proof and write the attestation in the SAR)
* Receive a new Attestation and its corresponding Badge on `0x2, mainnet.`

It means they can now use `0x2`to attest they have BAYC NFTs without leaking `0x1`
{% endhint %}

#### Attestations

```solidity
// Attestation format, context is the global attestation registry
struct Attestation
{
    address owner;          // The receiver of the attestation (0x2)
    uint256 chainid;        // The chainId of the network (1 for mainnet)
    address attester;       // address of the attester (ZK-SMPS Attester)
    uint256 collectionId;   // Id of the attestation collection 
                            //(BAYC Owners Attestations by ZK-SMPS Attester)
    Claim claim;            // underlying claim (owns BAYC NFT)
    bytes extraData;        // (optional) arbitrary data added by attester
}

```

#### Claims

A claim is a statement made by a user, backed by a proof that can be verified by an attester. \
Each attester has a set of claims that can be made by users.

```solidity
// Claim format, context is a specific attester (here ZK-SMPS Attester)
struct Claim 
{
    uint256 id;         // Identifier of the claim (
                        // Id = 3 <> Claim # 3 in ZK-SMPS Attester = BAYC ownership)
    uint256 value;      // value is the main data associated with the claim
                        // (e.g: 5 = owns 5 BAYC NFT)
    uint32 timestamp;   // timestamp of the claim (e.g time when proof was generated)
    bytes extraData;    // (optional) arbitrary data claimed
}
```

#### Sismo Attestations Registries (SARs)

Sismo Attestation Registries (SARs) are smart contracts deployed on EVM chains (and soon on other smart contracts platforms). They contain all attestations issued by the Sismo Protocol.

Attestations are regrouped under attestation collections.&#x20;

The protocol maintains a set of authorised attesters which are smart contracts allowed to write attestations Sismo Attestation Registries. Each attester are given a range of collection Ids they can write into.

```solidity
interface IAttestationsRegistry {
    // only authorized Attester can write attestations
    function writeAttestation(Attestation memory attestation) external;
    
    // function to authorize a new attester to write attestations in a specific range
    // of collections
    function authorizeRange(
        address attester,
        uint256 firstCollectionId,
        uint256 lastCollectionId
    ) external;
    
    function getAttestation(
        uint256 attestationCollectionId,
        address owner
    ) external view returns (Attestation memory);
}
```

#### Attesters

Attesters are smart contracts with write access to Sismo Attestation Registries. They verify user claims that are backed by proofs. They generate attestations from user claims.

```solidity
contract Attester {
    function attest(Claim memory claim, bytes calldata proofData) external {
        // optional (e.g check nullifier)
        _beforeAttest(claim, destination, proofData);
        // verify the proof against the claim (is 0x1 owner of >= 5 BAYC NFTs?)
        _verifyClaim(claim, destination, proofData);
        // generate the attestation from the claim
        Attestation memory attestation = 
            _constructAttestation(claim, destination, proofData);
        // writing in the attestation registry
        ATTESTATIONS_REGISTRY.writeAttestation(attestation);
        // optional (e.g register nullifier)
        _afterAttest(claim, destination, proofData);
    }
}
```

#### Proving Scheme

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

An Attester is a smart contract implementing the verifier of a claiming protocol to record attestations based on verified claims made by users, backed by the proofs they generated with the prover.&#x20;

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
* Develop and maintain new claiming protocols
* Develop tools around the Sismo Protocol

We are the core maintainers of Sismo Protocol. Its development and governance will be progressively handed to the Sismo DAO.

We will maintain and propose new Claiming Protocols to Sismo.

We will develop and improve the Sismo Wallet as well as libraries so Sismo attestations become easy to generate and use natively within apps.

## Sismo DAO

Sismo DAO is a **protocol DAO** launched in October 2021 to progressively oversee and manage the development and maintenance of Sismo Protocol. It started as a social DAO, gathering curated generations of members sharing similar interests about privacy, reputation, identity and zero-knowledge technologies.&#x20;

It is currently in charge of the allocation of Sismo DAO treasury and will move step by step towards an administrative role over Sismo Protocol.

{% content-ref url="broken-reference" %}
[Broken link](broken-reference)
{% endcontent-ref %}
