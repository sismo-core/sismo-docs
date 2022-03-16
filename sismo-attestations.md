---
cover: .gitbook/assets/TWITTER BANNERvert_1500x500px.jpeg
coverY: 0
---

# Sismo Attestations

Sismo protocol should allow anyone to generate attestations, easily integrable in their web2 and web3 applications.

Sismo is the host of several attestation protocols, authorized by Sismo DAO to write in the Sismo Attestations State (SAS).&#x20;

Each authorized attestation protocol gets a slot in the SAS.

While Sismo Attestations can originate from different attestation protocols they all share the following:

* They follow the Sismo Attestation standard format
* They live in Sismo Attestations State (SAS), the cross-chain database of all attestations.

#### Sismo Attestation Standard Format

```
// 
{
    attestationId // The identifier of the attestation
    destination:          // where the attestation will be written in the SAS
    attester:  // the origin of the attestation
    owner:   // the owner of the attestation
    timeStamp: // the date when the attestation has been verified
    value:     // the main value of the attestation (balance, score)
    extraAttestationData: // arbitrary data that has been attested by the attester
    attestationProof: // proof neeeded by external parties to verify validity


}
```

#### Sismo Attestation Destination

The destination is where the attestation will be effectively stored. This destination can be an onchain or offchain. \
\
The destination can be for instance the Sismo Attestations Smart Contract on mainnet:&#x20;

```
{
    attestationId: 5 // 5 <> Owns more than 10 Eth
    destination: SismoAttestations smart contract, on mainnet // Smart contracts that holds all attestations written on mainnet Ethereum
    owner: 0xc13..4b2
    attester:  ZKSAPAttesterAddress (ZKSAP is the attestation protocol)
    timeStamp: 1647420085 
    value:     1 ( 1 means yes, owns more than 10 ETH)
    extraAttestationData: EncodedsourcesNullifiers // Extra data attested by ZK-SAP attestation protocol
    attestationProof: null // not needed as anyone can verify ZKSAPAttester smart contract works as intended

}
```

Sismo will allow attestations to also be stored in other offchain databases.

```
{
    attestationId: 5 (5 <> Owns more than 10 Eth)
    destination: hostedDB.sismo.io // or ipfs/ ceramic
    owner: 0xc13..4b2
    attester:  api.sismo.io/attest/zk-sap (ZKSAP is the attestation protocol)
    timeStamp: 1647420085 
    value:     1 ( 1 means yes, owns more than 10 ETH)
    extraAttestationData: EncodedsourcesNullifiers // Extra data attested by ZK-SAP attestation protocol)
    attestationProof: signature of hosted zk-sap attestation api.

}
```

As of today, destinations are only EVM chains and attestations are written in SismoAttestations smart contracts deployed on these chains.

#### Attestations for web2 apps

In the future, Sismo will to make it easy for apps to use the attestation protocols themselves, verifying attestation proofs themselves and storing the attestation in their own database.

For a group chat app, the flow will be the following:

* To allow a user into a specific group chat, the chat app requires its users to meet a requirement (e.g own a BAYC NFT)
* The app redirect the Zikitor, Sismo's frontend
* In Zikitor, the user generates the attestation proofs needed
* The user is redirected to the chat app with its attestation proof
* The chat app checks the validity of the proof in its own backend (via an offchain attestor)
* Then if valid, the chat app car give access writes to the user.

