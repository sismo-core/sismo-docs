# Commitment Mapper

The Commitment Mapper, provided by Sismo, is a trusted offchain service housed in an isolated infrastructure. It allows account owners to transform proof of account ownership into proof of secret knowledge. The account owner receives a receipt from the Commitment Mapper, which connects their account to their commitment (e.g., the hash of their secret). This combination of the user's secret and the Commitment Mapper receipt forms the Delegated Proof of Ownership. The commitment can be used in zero-knowledge (ZK) systems as a [Vault or Proof Identifier](../../../build-with-sismo-connect/technical-documentation/vault-and-proof-identifiers.md).

## How It Works

* Users submit proof of account ownership (e.g., ECDSA signature for an Ethereum account, or OAuth verification for web2 accounts like GitHub, Twitter or Telegram) along with a commitment (e.g., the hash of a secret known only to them, or the EdDSA public key of one of their accounts).
* The Commitment Mapper verifies the validity of the account ownership proof and records the commitment for the account in a mapping stored in a database.
* If the account is already mapped with a commitment, the Commitment Mapper returns an error.
* The Commitment Mapper sends a signed commitment receipt to the user, confirming that they have completed the commitment process.
* Account owners can retrieve a commitment receipt at any time by providing new proof of ownership.

<figure><img src="../../../.gitbook/assets/commitment_mapper.png" alt=""><figcaption><p>Delegated Proof of Ownership workflow</p></figcaption></figure>

{% hint style="info" %}
Example: Using Hydra Delegated Proof of Ownership with the Commitment Mapper, the Poseidon hash function, and EdDSA digital signature schemes (which are SNARK-friendly), an Ethereum account owner and a secret commitment would follow these steps:

* The owner of 0x123..def signs a message to prove ownership.
* They send the signature along with the commitment (poseidonHash(secret)).
* The Commitment Mapper verifies the signature and registers the key-value pair: {key: 0x123...def, value: commitment = poseidonHash(secret)}.
* The Commitment Mapper signs the following receipt using its EdDSA private key: (poseidonHash(0x123...def, poseidonHash(secret)).

The user can then use this receipt to prove in a SNARK that they know the secret, went through the Commitment Mapper, and that their Commitment Mapper was linked.
{% endhint %}

## Why Use The Commitment Mapper?

In constrained environments like zk-SNARK circuits, traditional proofs of ownership become impractically expensive. Sismo alleviates this problem by using the Commitment Mapper signing with an EdDSA address, which is cheaper to prove and verify in a SNARK.

## Security Model

Users must trust the Commitment Mapper deployed by Sismo to:

* Verify proof of account ownership correctly.
* Authorize only one commitment per account.
* Keep the data store (set of account identifiers) private.

Sismo follows best practices with high web2 security in mind when deploying the Commitment Mapper:

* The Commitment Mapper is executed using an AWS Lambda deployed in an isolated infrastructure, within a dedicated AWS account.
* Private keys are stored using AWS KMS.
* Actions made inside the AWS account are traced, stored in another account, and cannot be modified, allowing for precise auditing.
* Alerts are sent to the entire Sismo team if actions occur inside this AWS account.

{% hint style="info" %}
The Commitment Mapper code and its terraform deployment are open-source and accessible here: [https://github.com/sismo-core/sismo-commitment-mapper](https://github.com/sismo-core/sismo-commitment-mapper).
{% endhint %}

## The Future

Sismo is developing a new version of the Commitment Mapper that will require less trust. Operations within the Commitment Mapper will be executed inside a SNARK and verified onchain.
