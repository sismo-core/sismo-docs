# Archive

### SMPS and ZK-SMPS

SMPS stands for Simple Merkle Proving Scheme, ZK-SMAP stands for Zero Knowledge Simple Merkle Attestation Protocol.

Both protocols consume account merkle trees which store in their leaves a key value database where the key is an eligible address and the value is a number. Merkle trees are maintained in a centralized yet transparent fashion, root are posted onchain.&#x20;

\[More on merkle trees)

\[Scheme: example top 1000 USDC Holders, score = 1 for top 1000, 2 for top 500, 3 for top 100]

Anyone owning an address in the tree can claim an attestation with a score inferior or equal to the value associated with they address in the tree.

SMAP protocol is public, the proving scheme is a simple merkle proof tested against the onchain root. The issued attestation is directly linked to its source of data.

ZK-SMAP does it in the Zero Knowledge way. It means that none can, from an attestation, trace back to the owned origin address. The underlying proving scheme is a merkle proof similar to SMAP but performed in a ZK SNARK.

SMAP is relatively cheap and can allow users to move data from an address to another without needing to move the underlying source of data. One can for instance generate from a cold wallet an attestation proving they are a top 1% AAVE holder and store it on a hot wallet. This enables them to use the hot wallet and its attestation to connect to gated tools without putting their hard assets at risk.

ZK-SMAP is a bit more expensive and guarantees total privacy. It is very interesting to aggregate reputation in a wallet without linking its sources or to l \[...]

\=> Link to SMAP, Link to ZK-SMAP
