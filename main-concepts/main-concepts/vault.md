---
description: A secure, sovereign and secret personal safe to store your source accounts
---

# Vault

The Sismo encrypted vault enables users to store their encrypted source accounts so that they can easily retrieve them when reconnecting to Sismo.\


### How does the vault work?

Every time a Sismo user connects to his destination account, a cryptographic key is generated using the signature made from this account. This key enables the user to unlock and manage the vault which is why the destination account is also called the vault controller address. When a connected user imports source accounts to Sismo, the proofs of ownership of these source accounts are automatically encrypted, linked to the user vault and saved in local storage.



Once you generate this key when creating your account, it is securely stored in a Ceramic stream (decentralized data storage protocol) that only you can control with your Sismo account controller address.

Every time you add a source or profile to your account, their proof of ownership will be encrypted thanks to this key and stored in a stream to access.

Every time you connect your Sismo account, the proofs of ownership of sources and profiles associated to it will be read from the stream and decrypted thanks to this key.

Your Sismo account is not stored in a centralized database, you own and control it.
