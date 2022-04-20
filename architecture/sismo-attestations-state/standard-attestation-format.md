# Standard Attestation Format

#### // TO BE UPDATED //

#### Standard Attestation Format

```
{
    owner: 0x123,  // the owner of the attestation
    value: 2,  // the main value of the attestation (balance, score)
    timestamp: 1650320924,  // the date when the attestation has been generated
    attester: // origin attester address
    extraAttestationData: [], // arbitrary data that has been attested by the atteste
}
```

#### Attestation Destination

The destination is where the attestation will be effectively stored. This destination can be an onchain or offchain. \
\
The destination can be, for instance, the Sismo Attestations Smart Contract on mainnet:&#x20;

```
{
    owner: 0xc13..4b2,
    value: 1, // ( 1 means yes, owns more than 10 ETH)
    timestamp: 1647420085, // the timestamp of the creation of the attestation
    attestationId: 5 // 5 <> Owns more than 10 Eth
    attestationDestination: { 
        databaseType: onchain, // the attestation is stored onchain
        chainId: 1 // the attestation is stored on the attestations.sol contract on mainnet
    },
    attestationOrigin: {  // the initiator of the attestation
        attester: ZK-SMPS attester, // The attestation was generated and validated using a zero knowledge proving scheme
        claimId: 5 // the claimId is the type of attestation. 
    },
    extraAttestationData: null, // the ZKSMPSAttester does not add extra attestation data.
    attestationProof: null // not needed as anyone can verify ZKSMPSAttester smart contract works as intended
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
