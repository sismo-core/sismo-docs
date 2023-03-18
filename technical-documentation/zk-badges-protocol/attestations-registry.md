# Attestations Registry

The **Attestations Registry** is the main smart contract of the Sismo protocol; it's in charge of **recording** all attestations issued by authorized issuers. Attestations are grouped into **Collections**.

An **attestation** is a certificate that proves some historical or reputational fact about a user.&#x20;

{% hint style="info" %}
Attestation Registries will be deployed on multiple chains with a focus on Ethereum L2s.&#x20;

There will be only one registry per chain maintained by the Sismo Protocol.
{% endhint %}

![An Attestations Collection in the Attestations Registry](<../../.gitbook/assets/4\_Attestations Registry.png>)

An attestation consists of:&#x20;

* <mark style="color:orange;">`owner: address`</mark> <mark style="color:orange;"></mark><mark style="color:orange;"></mark> - owner of the attestation (destination of the user request)
* <mark style="color:orange;">`value: uint256`</mark> - value of the attestation
* <mark style="color:orange;">`issuer: address`</mark> - issuer of the attestation (attester/ bridge/ migrator)
* <mark style="color:orange;">`timestamp: uint32`</mark> - (optional) timestamp of the validity of the underlying certificate issuance (can differ from recording timestamp e.g available group generation timestamp)
* <mark style="color:orange;">`extraData: bytes (optional)`</mark> - arbitrary data that can be added to the attestation by the issuer

### **Attestations Collection**

The Attestations Registry is split into **Attestations Collections**. A collection bundles owners that share some historical or reputation characteristics.&#x20;

Though multiple owners can have an attestation from the same collection (e.g. ENS DAO Voters), their attestations can differ by their value (Number of votes submitted) - or their _extraData_ ("last vote timestamp")

Only one attestation is stored on-chain per user per collection. A user can update an attestation, which will override the previous attestation.

{% hint style="success" %}
Anyone can integrate attestations in their on-chain or off-chain apps as a reputation tool, or to gate some of their features for specific users.
{% endhint %}

### Authorized Issuers

The **Sismo Governance** (currently a multisig owned by Sismo Core Team) is the owner of the Attestation Registry.

The Sismo Governance is the only entity allowed to authorize/unauthorize **issuers** to record attestations in the registry.

Issuers are authorized for a range of collectionIds, and effectively gain control of the attestations collections within their authorization range. They can record, update or delete any attestation in their controlled collection(s).

{% hint style="info" %}
Even though there is an authorization system in place, the Sismo protocol aims to be permisionless and decentralized.&#x20;

The issuers curation system is a net positive; it serves as a guarantee that the Attestations Registries are filled with high-value attestations and that external applications can consume them with trust.
{% endhint %}

![Governance and Attestations Registry](../../.gitbook/assets/5\_Governance.png)

The authorized **issuers** include attesters, bridges (which relay attestations between chains) and attestation operators, which create attestations derived from pre-existing attestations (e.g: you can claim the attestation #304 if you have attestations #1 and #2)

{% hint style="success" %}
Sismo registries should be thought of as cities.

* Issuers are land owners.
* Attestation owners are their inhabitants.
* Sismo Governance is the city council
* Sismo Protocol is the city administration, in charge of building infrastructure to attract new inhabitants
* Bridges and Attestations Operators are the infrastructure of the city, bringing more services to the inhabitants
{% endhint %}
