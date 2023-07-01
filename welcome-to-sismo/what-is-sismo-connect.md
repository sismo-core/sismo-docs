---
description: The crypto-native SSO.
---

# What Is Sismo Connect?

Sismo Connect, a crypto-native single sign-on (SSO), enables applications to request any data aggregated in a user’s Data Vault.

Onchain and offchain applications can request users to prove ownership of Data Sources (Ethereum, Twitter, Telegram and GitHub accounts) and Data Gems (e.g. that they own a certain NFT, contributed to a GitHub repository or voted in a DAO).&#x20;

Sismo Connect allows developers to implement Zero-Knowledge proofs in their applications with only a few lines of code.

<table data-view="cards"><thead><tr><th data-card-target data-type="content-ref"></th><th data-hidden data-card-cover data-type="files"></th></tr></thead><tbody><tr><td><a href="broken-reference">Broken link</a></td><td><a href="../.gitbook/assets/Build with Sismo Connect.png">Build with Sismo Connect.png</a></td></tr><tr><td><a href="https://apps.sismo.io">https://apps.sismo.io</a></td><td><a href="../.gitbook/assets/AppStore.png">AppStore.png</a></td></tr><tr><td><a href="https://case-studies.sismo.io">https://case-studies.sismo.io</a></td><td><a href="../.gitbook/assets/Case Studies.png">Case Studies.png</a></td></tr></tbody></table>

<figure><img src="../.gitbook/assets/Sismo Connect Flow (1).png" alt=""><figcaption></figcaption></figure>

## Apps can access powerful data at once

Apps can request at once:

* **Authentications:** Request users to prove ownership of Data Sources in their Data Vault. As such, your backend or smart contracts can easily authenticate that a user owns account:
  * Ethereum wallets
  * GitHub, Twitter or Telegram accounts
* **Selective disclosures:** Request the granular and anonymized data aggregated in a user’s Data Vault, called Data Gems. \
  Examples:
  * Minter of  "[Stand with Crypto](https://nft.coinbase.com/collection/ethereum/0x9d90669665607f08005cae4a7098143f554c59ef)" NFT  ([Data Group](https://factory.sismo.io/groups-explorer?search=stand-with-crypto-nft-minters))
  * Contributor to [Sismo Hub](https://github.com/sismo-core/sismo-hub) GitHub repo ([Data Group](https://factory.sismo.io/groups-explorer?search=sismo-hub-contributors-github))
  * Gitcoin Passport Holders ([Data Group](https://factory.sismo.io/groups-explorer?search=gitcoin-passport-holders))
  * [ENS DAO](https://docs.ens.domains/v/governance/) participant ([Data Group](https://factory.sismo.io/groups-explorer?search=ens-voters))
* **Signatures**: Your app can securely obtain user-signed messages, enabling front-running protection for instance.

## Case Studies

The case studies below showcase the power of Sismo Connect, demonstrating how it can benefit users and applications through concrete examples.

### SafeDrop: Sybil-Resistant Airdrop from Privately Aggregated Data

SafeDrop is a Sybil-resistant and privacy-preserving ERC20 airdrop that distributes AIR tokens to users proportionally based on their reputation, aggregated from diverse sources of data (EVM wallets, Telegram, Twitter and GitHub accounts).

<figure><img src="../.gitbook/assets/SafeDrop_Case Study_2 (3).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Learn how to build the Sybil-resistant airdrop [here](../build-with-sismo-connect/tutorials/tuto.md). Alternatively, check out the [onchain boilerplate](../build-with-sismo-connect/run-example-apps/onchain-sample-project.md).
{% endhint %}

By integrating Sismo Connect, SafeDrop users can generate a ZK proof to establish they own accounts making them eligible for the airdrop. No connection between these accounts and the airdrop destination address is ever made.

{% hint style="success" %}
Read the full case study [here](https://case-studies.sismo.io/db/safe-drop).
{% endhint %}

### Sybil-resistant and anonymous Lottery

Sismo built a "Privacy Is Normal" anon lottery. Only Tornado Cash users that have prove some proof of personhood via Gitcoin Passport can enter the lottery to win a printed tribute to the protocol’s revolutionary code or its NFT equivalent.

<figure><img src="../.gitbook/assets/Lottery Registration App_CASE STUDY.png" alt=""><figcaption></figcaption></figure>

By integrating Sismo Connect, Privacy Is Normal enables users to generate a ZK proof establishing they own a Tornado Cash depositing address without directly revealing it. In addition, users prove ownership of a Gitcoin Passport to add an effective layer of Sybil resistance, allowing only those users with adequate scores to participate.

{% hint style="success" %}
Read the full case study [here](https://sismo.notion.site/PROD-Sybil-resistant-anonymous-Lottery-gated-to-Tornado-Cash-users-1cdeef27f4d243f4a40c7aaa74e40ee9).
{% endhint %}
