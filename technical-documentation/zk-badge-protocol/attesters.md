# Attesters

**Attesters** are core smart contracts of the Sismo protocol. They are in charge of **verifying** user requests and **issuing** attestations. Example implementations can be found in the [Attesters Examples section](attesters-examples/).

![Attesters](<../../.gitbook/assets/3\_Attesters (1).png>)

#### Available Groups

A group is said to be **available** **for an attester** when the attester has access to on-chain data in the specific format that it requires. This enables the attester to validate user claims.\
\
Most attesters require a third party to post on-chain data (e.g merkle roots), though some just read from the EVM directly. Each available group has a group identifier so it can be targeted by user claims.

#### User Request&#x20;

A <mark style="color:orange;">`Claim`</mark>, in the context of a specific attester has three fields:

* <mark style="color:orange;">`groupId: uint256`</mark> - a targeted group from a list of groups available to the attester
* <mark style="color:orange;">`claimedValue:`</mark> <mark style="color:orange;">`uint256`</mark> - a claimed value to be compared to the actual account value that the user owns in a targeted group
* <mark style="color:orange;">`extraData: bytes`</mark> - optional arbitrary data that might be required and verified by the attester

A <mark style="color:orange;">`Request`</mark>, targeting a specific attester, is defined by:

* <mark style="color:orange;">`claims: Claim[]`</mark> - a set of user claims to be verified by the attester
* <mark style="color:orange;">`destination: address`</mark> - the address that will receive the attestations

For a user request to be validated by an attester, users must provide a <mark style="color:orange;">`proof`</mark> so that the attester can verify each claim.

The user sends the request (alongside its proof) to the attester by calling the <mark style="color:orange;">`generateAttestations(request, proof)`</mark>standard function of the attester contract.

### verifyRequest (internal standard function)

When the attester receives a user request, it first verifies whether the proof confirms the user's claim(s).&#x20;

This is the most important function of an attester.&#x20;

The standard internal function <mark style="color:orange;">`verifyRequest(request, proof)`</mark> of an attester takes the user request and its proof as arguments and throws if they are not valid.

While some attesters will simply perform checks (such as NFT/ERC20 balance check), others will do more complex proof verifications such as verifying a merkle proof or verifying a Zero-Knowedge Proof (ZKP).

{% hint style="info" %}
At Sismo we focus first on ZK Attesters, which are privacy-preserving attesters.&#x20;

Here, users provide proofs that **do not reveal** which accounts were used to confirm their claims.

In the verifyRequest function of our ZK Attesters, ZKP are verified.
{% endhint %}

{% hint style="success" %}
We invite developers to create innovative request verifications and issue creative attestations! As you'll see, the standard provides a lot of freedom for developers.
{% endhint %}

### buildAttestations (internal standard function)

Once a request has been verified, the attester must build the attestations that will be recorded in the Attestation Registry.

The internal function <mark style="color:orange;">`buildAttestations(request, proof)`</mark>of an attester is its second most important function.

This function takes the verified user request (its claims and destination) as input, and outputs the attestations that will be issued.

The built attestations are then recorded in the Attestation Registry.

### generateAttestations (external standard function)

```solidity
// This is the external function standard to all attesters
// This function is called by end users
function generateAttestations(Request calldata request, bytes calldata proofData)
    external
    override
    returns (Attestation[] memory)
  {
    // To be implemented by attesters devs
    // Verify if request is valid by verifying against proof
    _verifyRequest(request, proofData);

    // To be implemented by attesters devs
    // Generate the actual attestations from user request
    Attestation[] memory attestations = buildAttestations(request, proofData);

    // hook
    _beforeRecordAttestations(request, proofData);

    ATTESTATIONS_REGISTRY.recordAttestations(attestations);
   
   // hook
    _afterRecordAttestations(attestations);

    for (uint256 i = 0; i < attestations.length; i++) {
      emit AttestationGenerated(attestations[i]);
    }

    return attestations;
  }
```

{% hint style="info" %}
Head over to the [examples](attesters-examples/) section to see different implementations of attesters.
{% endhint %}

{% hint style="success" %}
We built the Sismo Protocol with modularity as top priority. Anyone is invited to propose new attesters by forking and tweaking existing ones, or by creating entirely new attesters! &#x20;
{% endhint %}
