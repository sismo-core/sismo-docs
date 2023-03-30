# Accounts Registry Tree

Accounts Registry Trees are used by some attesters as a data structure to store their available groups.

### Key-Value Merkle tree

A KV Merkle tree is a key-value store enhanced with a merkle tree. The merkle tree stores in its leaves the following data: hash(key, value).

### Accounts Tree

An Accounts Tree is a KV Merkle tree where the keys and values are accounts (e.g keys = account identifiers, values = accounts values)

This makes it easy and cheap for a verifier with access to the root of the tree (say a smart contract) to verify that a prover (a user) owns an account in the KV store.&#x20;

This prover just has to send the merkle proof to the verifier

![Accounts tree structure](<../.gitbook/assets/Merkle Tree1 (1).png>)

{% hint style="info" %}
An Accounts Tree can store a group of accounts.

An example of such group:

* account identifiers:  all addresses that voted in the ENS DAO
* account values: the number of submitted voted for each address
* (group identifier): 3

A user (owner of `0x123..def`) can easily prove to a smart contract (an attester with access to the root) that they voted 5 times.

They just need to provide the following information:

* Proof of `0x123..def` ownership: signature of a message with their private key
* 5: the account value
* merkle proof (data needed for the attester to reconstruct the accounts tree root)

The attester will then just need to:

* verify the signature
* compute the leaf = hash(`0x123..def,5)`
* Verify the merkle proof (hashing the leaf against the merkle proof elements to check whether it corresponds to the accounts tree root)
{% endhint %}

### Registry tree

The registry tree is another KV merkle tree where the key is an accounts tree root, and the value a specific value linked to the accounts tree (e.g for Hydra-S1 we chose the groupIndex as the accounts tree value)

![Registry tree structure](<../.gitbook/assets/Merkle Tree2.png>)

{% hint style="info" %}
Taking the previous example.&#x20;

A group of ENS Voters has a group identifier = 3

We can register the previous accounts tree of ENS Voters in the Registry tree with the value 3.

Users will then be able to prove to a verifier (that only has access to the registry tree root) that:

* They have an account that's part of one of the accounts trees registered
* The accounts tree they are a part of has the value of 3

\=> They will have effectively proved they are part of the group 3!
{% endhint %}

![Registry tree structure](<../.gitbook/assets/Merkle Tree3.png>)

### Hydra-S1 Attesters

Hydra-S1 Attester uses this scheme to enable users to prove they have accounts that are part of groups.

The proof of ownership is a bit more complex than signing a message, but the general flow is exactly the same.

The scheme is done in a ZK-SNARK which makes it impossible for anyone to know which account was used to prove that they are part of a group with a specific account!
