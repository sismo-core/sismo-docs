# ZK Badge Protocol

Sismo's ZK Badges protocol oversees the creation, updating, and deletion of **attestations**. An attestation is a 'certificate' showing that a user proved some fact about the current state or the history of their account(s).

The protocol is built on multiple modular concepts: [**Groups**](groups.md), [**Attesters**](attesters.md)**,** [**Attestations Registry**](attestations-registry.md)**,** and [**Badges**](badges.md)**.**

![Sismo Protocol](<../../.gitbook/assets/1\_Intro (2).png>)

Let's introduce the core modules of the protocol:&#x20;

1. **Groups of Accounts (Available Data)**: Data source for attestations
2. **Attesters (Smart Contracts)**: Issuers of attestations\
   \- _Verify Request_\
   \- _Build Attestations_\
   _- Generate Attestations_
3. **Attestations Registry (Smart Contract)**: Store of attestations\
   \- _Attestations Collection_\
   \- _Registry Governance_
4. **Badges (ERC1155):** Non Transferrable Token representation of attestations

Before presenting the protocol in detail, let's look at a simple example use case:

{% hint style="info" %}
Let's follow the user path of Alice, who wants to prove that she voted in the ENS DAO.

* Alice is part of a particular **group of accounts** (ENS DAO Voters)
* Alice interacts with an **Attester** (smart contract) to prove that she is **part of that group**
* Each attester has specific properties (e.g ZK or not) and requires specific **types of proofs** (e.g ZKP) from users
* Each attester needs access to the group data on-chain, in a **specific group format** (e.g Merkle tree root)
* Alice generates the proof in her browser on Sismo's frontend
* Alice sends the proof to the attester and receives an **Attestation** and a **Badge**
* The attestation is recorded in the **Attestations Registry** (smart contract)
* The **Badge** is a **Soulbound Token**
{% endhint %}

{% hint style="success" %}
The ZK Badges protocol is open-source and modular. It is a place that welcomes diverse contributors and allows them to build, modify and update the protocol together.
{% endhint %}

