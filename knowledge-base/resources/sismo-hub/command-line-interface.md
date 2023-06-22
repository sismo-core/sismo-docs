# Command Line Interface

The command line interface (CLI) is made of three commands.

### `generate-group`

The `generate-group` command is used to store a specific group of users in a JSON format in an off-chain database. You have to choose a valid [Group Generator](group-generators.md) to run this command.

```bash
# generate a group with the 'ens-voters' Group Generator
yarn generate-group ens-voters
```

The mandatory argument is a valid `generator-name`, i.e a valid [Group Generator](group-generators.md).

You then have the following options:

`--timestamp` : Pass a custom timestamp for your group generation

`--block-number` : Use a custom block number for your group generation

`--additional-data`: Add additional addresses and data in the generated group. Multiple addresses must be separated by a coma while addresses and data are separated by an equality sign.

```bash
yarn generate-group ens-voters --additional-data 0x2a3..ab4=2,0x1e5..th0=4
```



### `send-to-attester`

The `send-to-attester` command aims at sending generated groups on-chain. This command updates the on-chain [registry tree](../technical-concepts/accounts-registry-tree.md) by sending a new root.

```bash
# send groups on-chain for the 'hydra-s1-local' attester deployed on the 'local' chain
yarn send-to-attester hydra-s1-local local --send-on-chain

# send groups on-chain for the 'hydra-s1' attester deployed on the 'polygon' chain
yarn send-to-attester hydra-s1 polygon --send-on-chain
```

The two mandatory arguments for this command are:

`attester-name`: the name of your attester

`network`: the network where you attester is deployed

The following option is necessary to send the new registry root on-chain:

`--send-on-chain`: send available groups on-chain



### `api`

```bash
yarn api:watch
```

This command allows you to launch a local api at this url: [http://127.0.0.1:8000/static/rapidoc/index.html](http://127.0.0.1:8000/static/rapidoc/index.html)\
This swagger api can give information about:

* All the available data on the different chains, sorted by attesters&#x20;
* All the created badges on the different chains
* The metadata of a specific badge
* A list of all the flows&#x20;
* A list of all generated groups
* The latest groups generated
* The different group generators used
