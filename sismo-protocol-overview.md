---
cover: .gitbook/assets/TWITTER BANNERvert_1500x500px.jpeg
coverY: 0
---

# Sismo Protocol Overview

The Sismo Protocol is the set of rules linked to the creation, update and deletion of attestations recorded in Sismo Attestations Registries (SARs).

An **attestation** is a user **claim**, issued and verified by an **attester** (smart contract).

Attestation Registries feature **attestation collections.** A collection regroups owners of the same type of attestation, similarly to tokens in the ERC1155 NFT standard.

Sismo Governance is in charge of curating attesters that are allowed to record attestations in Sismo Attestations Registries.

{% hint style="info" %}
We will illustrate the following overview section with the example of a user who owns 5 BAYC NFTs on `0x1`.&#x20;

This user wants to generate an attestation of BAYC ownership, through our first authorized attester (It is called ZK-SMPS Attester, it is an attester that verifies user claims using ZK Proofs. It does not leak the source of data `0x1`).&#x20;

They want to receive the attestation on `0x2`, on Ethereum mainnet's Sismo Attestation Registry (SAR).

For this, the user will&#x20;

* Make a claim (claim of BAYNC NFT Ownership)
* Generate a proof from `0x1`(via the offchain ZK Snark prover of the ZK-SMPS Proving Scheme)
* Send this claim, along its proof to the attester (smart contract which verify the claim against the ZK Proof and record the attestation in the SAR)
* Receive a new attestation (and its corresponding NFT Badge) on `0x2, mainnet.`

They can now use `0x2`to attest they have BAYC NFTs without leaking `0x1!`
{% endhint %}

#### Attestations

```solidity
// Attestation format, context is the global attestation registry deployed on an EVM
struct Attestation
{
    address owner;          // The receiver of the attestation (0x2)
    (uint256 chainid;)      // Implicit: the chainId of the network (1 for mainnet)
    address attester;       // address of the attester which verified the claim
                            // and recorded the attestation (ZK-SMPS Attester)
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

The protocol maintains a set of authorised attesters which are smart contracts allowed to record attestations Sismo Attestation Registries. Each attester are given a range of collection Ids they can write into.

```solidity
interface IAttestationsRegistry {
    // only authorized Attester can record attestations in the registry
    function recordAttestation(Attestation memory attestation) external;
    
    // function to authorize a new attester to record attestations 
    // in a specific set of collections (range of collectionIds)
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

Attesters are smart contracts with write access to Sismo Attestation Registries. They verify user claims that are backed by proofs. They generate attestations from user claims and record them in attestations collections.

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
        // recording the attestation, writing in the attestation registry
        ATTESTATIONS_REGISTRY.recordAttestation(attestation);
        // optional (e.g register nullifier)
        _afterAttest(claim, destination, proofData);
    }
}
```

#### Proving Scheme

Let's take the ZKSMPS Attester as an example. This specific attester user ZK Proofs (generated via circom).

```solidity
constract ZKSMPSAttester is Attester {
    function _verifyClaim(
        Claim memory claim,
        address destination,
        bytes calldata proofData
    ) internal view virtual override {
        ZKSMPSProofData memory snarkProofData = abi.decode(proofData, (ZKSMPSProofData));

        // validates that snark input corresponds to the claim
        snarkProofData.input._validateInput(claim);
        snarkProofData._verifyCircomGroth16Proof(_verifier);
    }
}
```

#### Badges

Badges are the NFT representation of an attestation. The value of an attestation is encoded as the balance of the Badge.

```solidity
contract DefaultBadges is ERC1155 {
    IAttestationsRegistry immutable ATTESTATIONS_REGISTRY;

    function balanceOf(address account, uint256 id) public view virtual override returns (uint256) {
        return ATTESTATIONS_REGISTRY.getAttestation(id, account).claim.value;
    }
    // Badges.balanceOf(0x2, BAYCOwnershipCollectionId) = 5
    // Equivalent to 0x2 has the BAYCOwnership Attestation, with value 5
}so
```

{% content-ref url="broken-reference" %}
[Broken link](broken-reference)
{% endcontent-ref %}
