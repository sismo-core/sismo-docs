---
description: Attestation packaged as an NFT
---

# Badge

A Sismo badge is an attestation that has been packaged as an ERC-1155 NFT to be easily read by web3 applications and smart contracts. A badge can be linked to several different attestations allowing to obtain it.

By default (but it can be changed), Sismo badges are non-transferrable but they can be recovered by their original minter if they revoke the corresponding attestation(s), generate new one(s) linked to another destination controlled by the user and remint them. This makes Sismo Badges compliant to the ["Soulbound NFT"](https://vitalik.ca/general/2022/01/26/soulbound.html) concept.

ZK badges are comprised of the same information than the underlying attestation completed with:

* An illustration: the picture to be displayed by apps reading NFTs
* A description: short sentence describing the badge.
* Requirements: condition to be fulfilled to mint the badge
* A balance: value representing a score for some scored badges,
* An active duration: a time period for which the badge is considered valid and after which it can be renewed.

