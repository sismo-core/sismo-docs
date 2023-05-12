# Attesters Examples

The following examples are meant to provide a more tangible overview of the Sismo protocol.

We will illustrate the modularity of the protocol by introducing 4 different attesters, all recording in the Attestation Registry.&#x20;

![Authorized attesters](../../../../../.gitbook/assets/7\_Governance\_e.g.png)

The 4 attesters will need to be authorized by the Sismo governance to record in the Registry:

* ERC20 Holder Public Attester (authorized to write in collections from 10,000 to 20,000)
* ENS Holder Public Attester (20,000 to 30,000)
* Merkle Public Attester (30,000 to 40,000)
* Hydra-SA: Merkle ZK Attester (40,000 to 50,000)

The 4 attesters and the registry are deployed on the same chain. Feel free to head over to the [Attestations Registry](../attestations-registry.md) for more details about this step.



{% content-ref url="erc20-holder-public-attester.md" %}
[erc20-holder-public-attester.md](erc20-holder-public-attester.md)
{% endcontent-ref %}

{% content-ref url="ens-holder-public-attester.md" %}
[ens-holder-public-attester.md](ens-holder-public-attester.md)
{% endcontent-ref %}

{% content-ref url="public-merkle-attester.md" %}
[public-merkle-attester.md](public-merkle-attester.md)
{% endcontent-ref %}

{% content-ref url="hydra-s1-zk-merkle-attester.md" %}
[hydra-s1-zk-merkle-attester.md](hydra-s1-zk-merkle-attester.md)
{% endcontent-ref %}
