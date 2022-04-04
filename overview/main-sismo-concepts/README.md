---
description: Sismo For Dummies
---

# Sismo 101

## Sismo Overview



## How can Sismo preserve my privacy?

Using ZK-based claiming protocols, Sismo provides a privacy preserving way to generate attestations by breaking the on-chain link between source and destination accounts.&#x20;

It allows you to generate ZK proofs of claims (facts about your web3 accounts) locally and have them verified by a smart contract without revealing anything about the source fo the claims. The attestations generated after the verification are linked to a destination account and can be then minted as NFTs (ZK badges) to enable you to leverage that reputation in an anonymised manner.

However, if Sismo is used without protecting oneself upstream and downstream, there is no point and the anonymity would only be partial. To maximize privacy, several steps are recommended, such as:

* **Ensuring your destination account is not linked to personally identifiable information**\
  ****Make sure the destination account has been seeded with some native tokens for gas fees using Tornado Cash (or, if you trust a third party with that information, a centralized exchange) to prevent linking this account to another one of yours.\
  _Relayers to pay for gas will be explored in next iterations._
* **Distribute your attestations and badges on multiple destination accounts**\
  ****If you claim too many specific facts on a single destination account, it could be easy for anyone to identify you with a simple cross-checking of the claims.

{% hint style="warning" %}
**Example**\
"I own a CryptoPunk, I own more than 200 ETH and I submitted 3 proposals to Aave DAO."&#x20;

If you attest those _claims_ on a single destination account and those claims come from a single source account, it will be easy for anyone to locate that account by spotting the common addresses between those 3 sets.
{% endhint %}

*   **Using TOR and/or a VPN**

    Your internet service provider (ISP) identifies you with an IP address. To prevent third parties from knowing that you are using Sismo, you should consider using TOR and/or a VPN for your transfers. Avoid using free VPNs, they tend to keep or even sell your data. There are several VPNs on the market boasting a "no-log policy".
