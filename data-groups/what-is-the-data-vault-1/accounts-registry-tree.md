# Accounts Registry Tree

Accounts Registry Trees are used to structure data and store it onchain in the form of [Data Groups](broken-reference). As a result, users can participate in [proving schemes](../../data-vault/proving-schemes/) and prove facts about data aggregated in their [Data Vault](../../data-vault/what-is-the-data-vault.md).&#x20;

### Key-Value Merkle tree

A KV Merkle tree is a key-value database enhanced with a Merkle tree. The Merkle tree stores the following data: hash(key, value).&#x20;

### Accounts Tree

An Accounts Tree is a KV Merkle tree where the keys and values are accounts (e.g. keys = account identifiers, values = accounts values).

This makes it easy and cheap for a verifier with access to the root of the tree (e.g. a smart contract) to verify that a prover (a user) owns an account in the KV database. This prover just has to send the Merkle proof to the verifier.

![](<../../.gitbook/assets/Merkle Tree1 (3).png>)

{% hint style="info" %}
An Accounts Tree can store a group of accounts (i.e. a [Data Group](../data-groups-and-creation.md)).

For example:

* Account identifiers — all addresses that voted in the ENS DAO
* Account values — the number of votes submitted for each address
* (Group identifier): 3

A user (owner of `0x123..def`) can easily prove to a smart contract with access to the root) that they voted 5 times.

They just need to provide the following information:

* Proof of `0x123..def` ownership: signature of a message with their private key
* 5: the account value
* Merkle proof (data needed for the smart contract to reconstruct the Accounts Tree root)

The attester will then just need to:

* Verify the signature
* Compute the leaf = hash(`0x123..def,5)`
* Verify the Merkle proof (hashing the leaf against the Merkle proof elements to check whether it corresponds to the Accounts Tree root)
{% endhint %}

### Registry tree

The registry tree is another KV Merkle tree where the key is an accounts tree root, and the value is a specific value linked to the accounts tree (e.g. for Hydra-S1, we choose the groupIndex as the accounts tree value).&#x20;

![Registry tree structure](<../../.gitbook/assets/Merkle Tree2.png>)

{% hint style="info" %}
Taking the previous example.&#x20;

A group of ENS Voters has a group identifier = 3

We can register the previous accounts tree of ENS Voters in the Registry tree with the value 3.

Users will then be able to prove to a verifier (that only has access to the registry tree root) that:

* They have an account that's part of one of the accounts trees registered
* The accounts tree they are a part of has a value of 3

\=> They will have effectively proved they are part of group 3!
{% endhint %}

![Registry tree structure](<../../.gitbook/assets/Merkle Tree3.png>)

### Hydra Proving Schemes

Hydra [proving schemes](../../data-vault/proving-schemes/) use Account Registry Trees to enable users to prove they are part of a [Data Group](../data-groups-and-creation.md). Generating proof of ownership is more complex than simply signing a message, but the general flow is exactly the same. As the scheme is done in a zk-SNARK, it is impossible for anyone to know which account was used.
