# Data Sources & Data Groups

## Data Sources

Data Sources are the accounts that users aggregate in their Data Vault.&#x20;

From imported Data Sources, vault owners can generate ZK Proofs of:

* ownership of a specific Data Source (e.g authentication)
* inclusion of an owned Data Source in a Data Group (+ claim about its value in the group)

| Currently supported Data Sources |
| -------------------------------- |
| Ethereum wallets/ EVM accounts   |
| GitHub accounts                  |
| Twitter accounts                 |
| Telegram accounts                |

## Data Groups

Data Groups are sets of Data Sources where each Data Source has an associated value.

| Data Group Examples                                                                                                   | Members (Data Sources)                                                         | Value for each Data Source               |
| --------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------ | ---------------------------------------- |
| ["Stand With Crypto" NFT Minters](https://factory.sismo.io/groups-explorer?search=0xfae674b6cba3ff2f8ce2114defb200b1) | wallets of minters                                                             | number of NFT minted                     |
| [Gitcoin Passport Holders](https://factory.sismo.io/groups-explorer?search=0x1cde61966decb8600dfd0749bd371f12)        | wallets of Gitcoin Passport holders                                            | sybil-resistant score                    |
| [Sismo Hub Github Contributors ](https://factory.sismo.io/groups-explorer?search=0xda1c3726426d5639f4c6352c2c976b87)  | github accounts of contributors to sismo-core/sismo-hub repo                   | number of contributions                  |
| [ENS DAO Voters](https://factory.sismo.io/groups-explorer?search=0x85c7ee90829de70d0d51f52336ea4722)                  | wallets of voters in ENS DAO                                                   | number of votes                          |
| [Sismo Community Members](https://factory.sismo.io/groups-explorer?search=0xd630aa769278cacde879c5c0fe5d203c)         | wallets, github, telegram and twitter accounts of all people that helped Sismo | level of their contributions (1, 2 or 3) |

a

<details>

<summary>Example of a simple Data Group</summary>

```
// Some code
```

</details>



users can generate ZK Proof of:&#x20;

* Data Source ownership, enabling to authenticate themselves to applications via Sismo Connect
* Inclusion of a Data Source in a Data Group, with some claim about the associate
*
