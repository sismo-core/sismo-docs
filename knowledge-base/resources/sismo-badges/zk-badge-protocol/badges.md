# Badges

When a user receives an attestation, they also receive a **Badge** that wraps the attestation data in a **Non-Transferable Token a.k.a Soulbound Token** (non-transferable ERC1155). The value of the attestation is encoded as the balance of that ERC1155 NFT.

{% hint style="info" %}
Note: The Badge can be considered 'soulbound' to the source account used to confirm the user claim(s) and generate the attestation and badge.&#x20;

The Hydra-S1 Soulbound Attester, for instance, allows users to update the owner of their attestation and thus their badge owner.
{% endhint %}

Badges make Sismo attestations natively readable by all apps supporting the ERC1155 NFT standard such as Snapshot, Guild and countless other apps!

![Attestation <-> Badge](../../../../.gitbook/assets/6\_Badges.png)

The user balance for a specific badge is always equal to the value of its underlying attestation. In fact, Badges are a stateless ERC1155 smart contract and balances are read directly from the underlying attestation's value:

```solidity
function balanceOf(address account, uint256 id) public view returns (uint256) {
    return ATTESTATIONS_REGISTRY.getAttestationValue(id, account);
  }
```

{% hint style="info" %}
Example: Alice owns the Attestation #135 with value = 4 and also owns the Badge #135 with balance = 4.\
(e.g the destination address <mark style="color:red;">`0x3a..7e`</mark> owns 4 ZK Badges:  "ENS DAO Voter" )
{% endhint %}

{% hint style="success" %}
Anyone is welcome to create custom badges that read attestations!
{% endhint %}
