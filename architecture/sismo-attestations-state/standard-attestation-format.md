# Standard Attestation Format

#### Standard Attestation Format

```
// 
{
    attestationOrigin: {
        attester: ZK-SEP attester
        claimId: 5 
    }
    attestationDestination: {
        databaseType: onchain
        chainId: 1
    }
    attestationData: {
        timeStamp
        value
        
    }
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
