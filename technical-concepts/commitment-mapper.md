# Commitment Mapper

The commitment mapper is an off-chain trusted service provided by Sismo. It is immutable in an isolated infrastructure.

It enables account owners to convert a proof of account ownership into a proof of secret knowledge.&#x20;

The account owner receives a receipt from the Commitment Mapper, which maps their account with their commitment (e.g hash of their secret).

The combination of the user's secret and the commitment mapper receipt form the **Delegated Proof Of Ownership.**

{% hint style="info" %}
The commitment can be used in ZK systems as an account nullifier
{% endhint %}



### How it works

Users submit a proof of account ownership (e.g ECDSA signature from a wallet for an Ethereum account or OAuth verification for a web2 account like GitHub or Twitter account) to the commitment mapper alongside a commitment (e.g hash of a secret only they know or the EdDSA public key of an account of theirs).&#x20;

The commitment mapper will verify the validity of the account ownership proof and register the commitment for this account in a mapping stored on a database.&#x20;

If the account is already mapped with a commitment, the commitment mapper will return an error.&#x20;

Next, the mapper will send a signed commitment receipt to the user, which guarantees that they went through the commitment scheme.

A commitment receipt can be retrieved at any time by the account owner by sending a new proof of ownership.

<figure><img src="../.gitbook/assets/commitment_mapper.png" alt=""><figcaption><p>Delegated Proof of Ownership workflow</p></figcaption></figure>

{% hint style="info" %}
Example: Hydra Delegated Proof of Ownership via Commitment Mapper which uses Poseidon hash function and EdDSA digital signature schemes which are SNARK-friendly

For an Ethereum account and a secret commitment:

* The owner of `0x123..def` will sign a message to prove ownership of the account
* They will send the signature along with its commitment = poseidonHash(secret)
* The commitment mapper will verify the signature and then register the following key: value
  * key: 0x123...def
  * value: commitment = poseidonHash(secret)
* The commitment mapper will sign (with its EdDSA private key) the following receipt: (poseidonHash(0x123...def, poseidonHash(secret))

Note: The user will be able to use this receipt to prove in a SNARK that:&#x20;

1. They know the secret (they generate poseidonHash(secret) in the SNARK)
2. They went through the commitment mapper (verify the commitment mapper EdDSA signature in the SNARK)
3. Their commitment mapper was linked (verify the receipt construction in the SNARK)
{% endhint %}

#### Why?

Proving the ownership of an Ethereum account to a third party is generally simple via a message signature. Similarly, one can easily prove ownership of a Github or Twitter account via OAuth.

However, in constrained environments like ZK-SNARKs circuits, those proofs of ownership become impractically expensive.&#x20;

For instance, the ECDSA signature of Ethereum accounts are very expensive to prove inside a SNARK. By using a Commitment Mapper signing received with an EdDSA address (cheap to prove and verify in a SNARK), Sismo alleviates this problem.&#x20;

Also, the ECDSA signature cannot be used as a nullifier because it is malleable (several different valid signatures can be created using ECDSA). By enforcing a single commitment for an Ethereum account, the secret (only known to the user) that generated the commitment can be used as a nullifier.

#### Proof of Ownership Independence

It is interesting to note that compared to semaphore which also uses commitments, the commitment mapper only verifies proofs of ownership.

This means that a Delegated Proof of Ownership can be reused in any system.

For instance, in Sismo Hydra-S1 Attesters, the same commitment can be used to prove group membership in any group of accounts.&#x20;

The group data (e.g the list of accounts) is independent from the proof of ownership.

#### Off-chain Commitments

By keeping the mapping off-chain, the Commitment Mapper ensures that the set of all the accounts that interacted with it (called the Anonymity Set) remains private.

Other projects such as semaphore or tornado cash use the same kind of commitments (though they link the data to it) but they do it publicly, creating anonymity sets.

There is a path to more openness on our side: once the anonymity set is big enough, we will be able to publish our exact data.



### Security model

Users need to trust the commitment mapper deployed by Sismo to:

* Verify proof of account ownership correctly
* Authorize only one commitment per account
* Keep the data store (set of account identifiers) private

We deployed the commitment mapper with best practices and high web2 security in mind:

* The commitment mapper is executed using an AWS lambda deployed in an isolated infrastructure. A entire AWS account is dedicated to hosting the commitment mapper.
* The private keys (e.g EdDSA private key to sign commitment receipts for Hydra) are stored using AWS KMS.
* All actions made inside this AWS account are traced and stored on an other account. These traces can't be modified. That means we can audit exactly what happened inside this AWS account.
* Alerts are sent to the entire Sismo team in case of actions made inside this AWS account.

The code (along its terraform deployment) for the commitment mapper is open source and accessible here [https://github.com/sismo-core/sismo-commitment-mapper](https://github.com/sismo-core/sismo-commitment-mapper).



### Future of the commitment mapper

We are developing another version of the commitment mapper that will require less trust towards Sismo.&#x20;

Operations made inside the commitment mapper will be executed inside a SNARK and verified on-chain.
