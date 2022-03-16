---
cover: .gitbook/assets/TWITTER BANNERvert_1500x500px.jpeg
coverY: 0
---

# Sismo Attestations

Sismo protocol should allow anyone to generate attestations, easily integrable in their web2 and web3 applications.

Sismo is the host of several attestation protocols, each authorized by Sismo DAO, each with an allocated slot in Sismo Attestations State, and each with a new set of properties regarding decentralization, privacy and correctness.

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
    extraAttestationData: "Attested by ZK-SAP Attestation Protocol: has more than 10 ETH"
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
    extraAttestationData: "Attested by ZK-SAP Attestation Protocol: has more than 10 ETH"
    attestationProof: signature of hosted zk-sap attestation api.

}
```

As of today, destinations are only EVM chains and attestations are written in SismoAttestations smart contracts deployed on these chains.

#### Attestations for web2 apps

In the future, Sismo will to make it easy for apps to use the attestation protocols themselves, in their own backend. \
\
Their frontend would redirect to Zikitor, Sismo frontend, where user will generate the attestation proofs from their source accounts. \
Zikitor redirects back to the app with the attestationProof that the app can verify and attest in its own server.

The destination could also well be a web2 application itself, or an OAuth system for web2 applications.

```
{
    attestationId: 5 (5 <> Owns more than 10 Eth)
    destination: api.externalApp.com/verifier
    owner: 0xc13..4b2
    attester:  externalApp
    timeStamp: 1647420085 
    value:     1 ( 1 means yes, owns more than 10 ETH)
    extraAttestationData: ""
    attestationProof: 

}
```

#### Sismo Attestion State

\[Scheme du SAS carré]

It is the global state of all attestations generated through Sismo protocol.

It is effectively a crosschain aggregated database, maintained by the Sismo protocol. \
It is the union of all SismoAttestations smart contracts deployed on different EVM chains and other offchain databases maintained by Sismo.

