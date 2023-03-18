---
description: Frequently asked questions
---

# Privacy & Security FAQ

### What data is needed to mint a ZK Badge, and how is privacy ensured?

To mint a ZK Badge using Sismo, you generate a zero-knowledge proof that verifies your account is a member of the ZK Badge’s group of eligible accounts.&#x20;

When generating your ZK proof, you provide the protocol with a Merkle proof—verifying that your account is part of a Merkle tree and, therefore, a member of the group.&#x20;

To make the computation of Merkle proofs possible, the Merkle tree is downloaded in your browser. This prevents accounts inside the Merkle tree from being leaked via server requests.

### How do we know if a ZK Badge has already been minted and is no longer mintable?

Each ZK Badge has its own nullifier—a deterministic number derived from the ZK Badge’s source account. Only the owner of a nullifier’s source account can compute it.&#x20;

This nullifier is stored on-chain to ensure a single source account can not mint duplicate ZK Badges. If a nullifier for a specific ZK Badge and source account is already present on-chain, additional Badges cannot be minted.&#x20;

To prevent the possibility of linking a user’s destination accounts via correlated nullifiers, the entire set of used nullifiers is downloaded in a user’s browser. Utilizing a subgraph and public RPC endpoint, an RPC listener uses eth\_getLogs to periodically poll the most recent on-chain nullifiers.

### How do we check the eligibility of a ZK Badge without sending the account through an API call?

To check the eligibility of a ZK Badge, you need to find out if the account is part of the corresponding group of eligible accounts.&#x20;

If you want to avoid sending your address to an API, you can download the entire group of accounts in your browser to check if yours is present.&#x20;

### How do you ensure OAuth providers such as GitHub do not correlate logs?

Users create connection logs when importing web2 accounts into their Sismo Vault. As OAuth depends on third parties outside of Sismo’s control, we have no way of knowing what providers such as GitHub do with their logs.

<figure><img src="https://lh6.googleusercontent.com/to5Md0yLeQLR30R5OOkh20mvh1yoHDX8HaXFSBUCxRuq_MtSeUY9YWrF-pHo0KoPXGmAMDQ4N1MRvXIgqtGSlSwN4vOPDZ-CYKKexwnb0hGFQXWQPXZvO07mCC6ZSDuo5ljDZpdYYJNM08c5O9WG9ieSFdHc-GjebbghrpqmkQ1v-CF26EVaSQxB_M1zdA" alt=""><figcaption></figcaption></figure>

For this reason, we recommend that users do not immediately mint a ZK Badge after importing web2 accounts into their Vault. This ensures a much higher level of privacy via a larger anonymity set.&#x20;

### Why do we need to import the destination account into the Sismo Vault?

When minting a ZK Badge, you prove that you own both the source and destination account. This ZK proof is submitted to an attester that issues a ZK Badge to the destination account.

The destination account must be imported into the Vault to go through the commitment mapper process. The commitment mapper verifies proof of ownership and privately associates a single address with a commitment.&#x20;

Users only need to go through this process once—when they first import an account into their Sismo Vault.&#x20;

### Are accounts sent to a server when minting a ZK Badge?

No. Users generate the proofs necessary to mint ZK Badges entirely in their browser thanks to the Sismo Vault.&#x20;

The Sismo Vault stores imported accounts and cryptographic secrets, signatures, commitments, and receipts from the commitment mapper. It only ever exists in its decrypted state in a user’s browser.&#x20;

### Are accounts sent to a server when they are imported into the Sismo Vault?

Yes. Accounts imported into the Vault need to go through the [commitment mapper](https://docs.sismo.io/sismo-docs/technical-concepts/commitment-mapper)—an off-chain service. This process only needs to be done once when first importing accounts into the Vault.

For a maximum level of privacy, we recommend waiting between importing accounts into the Vault and minting ZK Badges. A larger anonymity set ensures no correlation can be made.&#x20;

### Do you store links between source accounts and destination accounts?

No. When minting a ZK Badge, users generate a nullifier that is stored on-chain. This nullifier is deterministically derived from the user’s source account and prevents the user from minting duplicate ZK Badges.&#x20;

Nobody can use a nullifier to infer the origin of a ZK Badge. It can only be computed by the associated source account’s owner.

### How is the Sismo Vault stored?

The Sismo Vault is analogous to an encrypted password manager. It only ever exists in its decrypted state in a user’s browser—remaining fully encrypted in an off-chain key-value store API.&#x20;

### The commitment mapper is an off-chain trusted service. What does this mean for privacy?

Currently, verifying ECDSA signatures in a zk-SNARK circuit is problematic and would require the following:

* Having a user’s private key to deterministically compute a nullifier (ECDSA is not deterministic)
* Having a much lower number of constraints to run sufficiently fast on a browser and guarantee privacy

To bypass these issues, Sismo has developed an off-chain commitment scheme that converts proof of account ownership into proof of secret knowledge. Users only need to go through the commitment scheme once when importing new accounts into the Sismo Vault.&#x20;

You can read more about the commitment mapper here: [https://docs.sismo.io/sismo-docs/technical-concepts/commitment-mapper](https://docs.sismo.io/sismo-docs/technical-concepts/commitment-mapper)

### What if the commitment mapper gets hacked?

In the event the commitment mapper was hacked, the set of all accounts used in the commitment scheme would be leaked. In privacy terms, this data is called an anonymity set.&#x20;

The larger an anonymity set, the more difficult it is to infer anything from the data inside. Currently, the commitment mapper’s anonymity set exceeds 100,000 accounts—making it very difficult to correlate.

### What is the security model of the commitment mapper?

Currently, users need to trust the commitment mapper to do the following:

* Correctly verify proof of account ownership
* Authorize only one commitment per account
* Keep the data set private&#x20;

It is hosted using AWS Lambda in an isolated environment. An entire AWS account is dedicated to hosting the commitment mapper. We deployed the commitment mapper with best industry practices and maximum web2 security in mind:

* Private EdDSA keys used to sign commitment receipts are stored using AWS KMS.
* All activity made on the AWS account is traced and stored on a separate account. As this is unmodifiable, we can audit all activity on the AWS account.&#x20;
* Alerts are sent to the entire Sismo team if actions are made on the AWS account.&#x20;
* The code (along with its terraform deployment) for the commitment mapper is open-source and accessible [here. ](https://github.com/sismo-core/sismo-commitment-mapper)

### What are the plans for the commitment mapper?

We are currently working on a solution that requires users to trust the commitment mapper much less. \
