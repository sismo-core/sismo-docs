# ERC20 Holder Public Attester

The ERC20 Holder Public Attester is a naive public attester that issues attestations to users based on their current ERC20 balances.&#x20;

![ERC20 Holder Public Attester](<../../../../../.gitbook/assets/1 (4).png>)

This attester will check whether a user currently holds more than a claimed amount of an ERC20 token, and issue an attestation that proves it.

### Available groups

Since the attester is simply checking balances directly on-chain, the available groups of accounts are all ERC20 Tokens holders of ERC20 contracts deployed on the chain of said attester.

The natural identifier of the group is the address of the ERC20 token.&#x20;

{% hint style="info" %}
e.g. a group of ENS holders is the following:

* groupId: 0xc18360217d8f7ab5e7c516566761ea12ce7f9d72 (address of ENS ERC20 Token)
  * accounts: owners of ENS&#x20;
  * accounts values = balances of ENS tokens for each owner
{% endhint %}

### Claims supported by the ERC20 Holder Public Attester

Users can make claims of being part of a group of ERC20 token holders. They can claim to have more than a claimed value of tokens.

{% hint style="info" %}
User claim:

* groupId: ERC20 address of a token
* claimedValue: 10 (user claims to have more than 10 tokens)
{% endhint %}

### Verify request function

This attester will verify whether the user has a balance greater than what they claim.&#x20;

Proof is empty - the user has proved ownership of the account by sending a transaction from this account. The user has proved group membership and account value directly from on-chain data.

```solidity
function _verifyRequest(Request calldata request, bytes calldata proofData) internal virtual override {
    Claim memory claim = request.claims[0];

    address tokenAddress = address(uint160(claim.groupId));

    uint256 tokenBalance = ERC20(tokenAddress).balanceOf(msg.sender);

    if (tokenBalance < claim.claimedValue) // an alternative could check strict equality
      revert ClaimValueInvalid(tokenBalance, claim.claimedValue);
  }
```

{% hint style="info" %}
We could also have checked a strict equality or any other condition.
{% endhint %}

### Build attestations function

Once the claim is verified (e.g. a user has proved to have a balance greater than its claimed value), the attester will generate an attestation from a collection that groups all owners of the same token:

```solidity
  // _collectionsInternalMapping(AAVEAddress) = 0;
  // _collectionsInternalMapping(ENSAddress) = 1;
  // _collectionsInternalMapping(OPAddress) = 2;
  // _collectionsInternalMapping(UNIAddress) = 3;
  // _collectionsInternalMapping(WETHAddress) = 4;
  mapping(address => uint256) internal _collectionsInternalMapping;

  function buildAttestations(Request calldata request, bytes calldata)
    public
    view
    virtual
    override(IAttester, Attester)
    returns (Attestation[] memory)
  {
    Attestation[] memory attestations = new Attestation[](1);

    Claim memory claim = request.claims[0];

    address ERC20Address = address(uint160(claim.groupId));

    // For the ENS holder:
    // 10 000 + 1, this attester is authorized from 10 000 to 20 000
    uint256 attestationCollectionId = AUTHORIZED_COLLECTION_ID_FIRST +
      _collectionsInternalMapping(ERC20Address);

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

And we are done coding our simple attester!

#### Limitations

{% hint style="info" %}
* This attester creates an immutable link between the source account and the destination.
  * Improvement: do it in ZK :)
* This attester can be used to send attestations to a destination that did not consent
  * Improvement: add proof of ownership for the destination
* This attester is not entirely permissionless; it requires each group to be registered in `_collectionsInternalMapping`
  * Improvement: find a way to create a unique collectionId within the authorized collections Ids generated from an address.
{% endhint %}

{% hint style="success" %}
This is a simple example used to demonstrate our attester standard - please open a [PR](https://github.com/sismo-core/sismo-protocol/tree/main/contracts/attesters) with an improved version of it!
{% endhint %}

Next, let's take a look at a small variation of this model: ENS Holder Public Attester.
