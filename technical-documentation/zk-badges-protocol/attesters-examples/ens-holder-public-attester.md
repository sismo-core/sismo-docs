# ENS Holder Public Attester

The ENS Holder Public Attester issues attestations to ENS owners based on their current ENS balance, by using the following rule(s):&#x20;

* If 0 < user ENS balance ≤ 10 => get an attestation
* If 10 < user ENS balance ≤ 100 (10^2) => get another attestation
* If 10^2 < user ENS balance ≤ 1000 (10^3) => get another attestation
* ...

![ENS Holder Public Attester](<../../../.gitbook/assets/2 (2).png>)

### Available groups

The available groups of accounts are all current holders of the ENS token, split in slices.

{% hint style="info" %}
e.g. the available groups are as follows:

* groupId: 0
  * account identifier: address of ENS owners with 0 < ENS balance ≤ 10&#x20;
  * account value: balance
* groupId 1
  * account identifier: address of ENS owners with 10 < ENS balance ≤ 10^2&#x20;
  * account value: balance
* ...
{% endhint %}

### Claims supported by the ERC20 Holder Public Attester

Users can make claims of being part of one specific group of ENS token holders.&#x20;

{% hint style="info" %}
User claim:

* groupId: 2
* claimedValue: 198
{% endhint %}

### Verify request function

This attester will check whether the user has the balance they claim and if it corresponds to the groupId.

The proof is empty - the user has proved ownership of the account by sending a transaction from this account. The user has proved group membership and account value directly from on-chain data.

```solidity
function _verifyRequest(Request calldata request, bytes calldata proofData) internal virtual override {
    Claim memory claim = request.claims[0];

    uint256 tokenBalance = ENS.balanceOf(msg.sender);

    if (tokenBalance <= 10**claim.groupId) revert NotInGroup(claim.groupId);
    if (tokenBalance > 10**(claim.groupId+1) ) revert NotInGroup(claim.groupId);
    if (tokenBalance != claim.claimedValue) 
      revert ClaimValueInvalid(tokenBalance, claim.claimedValue);
  }
```

### Build attestations function

Once the claim is verified (e.g. a user has proved to have the correct balance), the attester will generate an attestation from a collection that groups all owners of ENS balances in the same range!

```solidity
  function buildAttestations(Request calldata request, bytes calldata)
    public
    view
    virtual
    override(IAttester, Attester)
    returns (Attestation[] memory)
  {
    Attestation[] memory attestations = new Attestation[](1);

    Claim memory claim = request.claims[0];
    // 20 000 + 2, this attester is authorized from 20 000 to 30 000
    uint256 attestationCollectionId = AUTHORIZED_COLLECTION_ID_FIRST +
      claim.groupId;

    address issuer = address(this);

    attestations[0] = Attestation(
      attestationCollectionId,
      request.destination,
      issuer,
      claim.claimedValue,
      block.timestamp,
      ''
    );
    return (attestations);
  }
```

#### Limitations

{% hint style="info" %}
* This attester creates an immutable link between the source account and the destination.
  * Improvement: do it in ZK :)
* This attester can be used to send attestations to a destination that did not consent
  * Improvement: add proof of ownership for the destination
{% endhint %}

This is a simple example used to demonstrate our attester standard - please open a [PR](https://github.com/sismo-core/sismo-protocol/tree/main/contracts/attesters) with an improved version of it!

Next, onto something a bit more complex.
