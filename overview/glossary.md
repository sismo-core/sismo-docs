---
description: A list of terms related to Sismo and their definitions
---

# Glossary

| Term                                                                             | Definition                                                                                                                                                                                                                                                                                   |
| -------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Attestation**                                                                  | An Attestation is a claim (linked to the history of one or multiple source accounts) that has been attested by Sismo protocol. It can be packaged as a NFT to become a ZK badge or be read off-chain directly by a third party.                                                              |
| **Destination Account**                                                          | A Destination Account is an Ethereum account controlled by the user that they will use to store the badges they will mint.                                                                                                                                                                   |
| **Profile**                                                                      | A Sismo Profile is a view of of a user’s destination account displaying the badges he has minted and other details.                                                                                                                                                                          |
| **Source Account**                                                               | A Source Account is an Ethereum account controlled by the user that they can use as a source of data to attest they meet the conditions required to mint a specific badge.                                                                                                                   |
| **Source Account Signet**                                                        | A Source Account Signet is a proof of ownership of a source account. It is generated when a user imports a source account and is used to attest a user owns a source every time they mint a ZK badge.                                                                                        |
| **Vault**                                                                        | A Sismo Encrypted Vault enables users to store their encrypted source accounts so that they can easily retrieve them when reconnecting to Sismo.                                                                                                                                             |
| **Vault Controller**                                                             | A Sismo Vault Controller is the address from which a user generates its vault key. In the current version, this address is the Destination account for the ZK badges.                                                                                                                        |
| **Vault Key**                                                                    | A Sismo Vault Key is a cryptographic key generated from a signature made from the user’s vault controller EVM address. It allows the user to unlock their encrypted vault and use the source accounts stored in it to mint badges.                                                           |
| **ZK Badge**                                                                     | A ZK Badge is a zero knowledge proof of a fact (linked to the history of one or multiple source accounts) that has been attested and then packaged as a NFT by Sismo protocol.                                                                                                               |
| <p><strong>ZK Badge</strong> <br><strong>Active Duration</strong></p>            | A ZK Badge Active Duration is an optional badge parameter that define for how long the badge will be valid after it has been minted. Once the active duration is over, the balance of the ZK badge is set to 0 and its owner would have to renew it to keep using it.                        |
| **ZK Badge Description**                                                         | A ZK Badge description is a brief sentence describing a ZK badge.                                                                                                                                                                                                                            |
| <p><strong>ZK Badge Last</strong> <br><strong>Attestation Timestamp</strong></p> | A ZK Badge Last Attestation Timestamp is the time at which the ZK badge was either minted or last renewed. It enables any application reading a ZK badge details to know when the proof was attested and to compute if the ZK badge is expired or not according to the application’s policy. |
| **ZK Badge Requirement**                                                         | A ZK Badge Requirement is a detailed condition a user needs to fulfill with his selected source accounts to be able to mint a specific ZK badge.                                                                                                                                             |
| **ZK Badge Score**                                                               | A ZK Badge Score is a number attributed to a ZK badge instance minted by a user taking into account several parameters monitored from the source accounts selected.                                                                                                                          |
| **ZK Snark**                                                                     | A ZK-SNARK is a cryptographic proof that allows one party to prove it possesses certain information without revealing that information and without any interaction between the prover and verifier. it stands for “Zero-Knowledge Succinct Non-Interactive Argument of Knowledge.”           |
