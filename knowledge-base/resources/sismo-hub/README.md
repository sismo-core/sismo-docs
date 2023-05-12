# Sismo Hub

You can find in this section all the technical documentation of the [Sismo Hub repository](https://github.com/sismo-core/sismo-hub), the repository used for integrations on Sismo.\
\
The Sismo Hub is powered by an offchain infrastructure which:

* Manages Groups: The infrastructure periodically generates off-chain Groups that aim to be reusable and sent on-chain for attesters like the [HydraS1SimpleAttester](https://github.com/sismo-core/sismo-protocol/blob/main/contracts/attesters/hydra-s1/HydraS1SimpleAttester.sol). A Group of accounts bundles accounts that share some reputational or historical characteristics. Anyone can propose a new group to Sismo. The infrastructure will send the groups on-chain to the right attester so your generated group becomes the eligible group for a specific badge.
* Manages Badges metadata.&#x20;
