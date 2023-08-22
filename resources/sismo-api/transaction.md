---
description: Ethereum transaction
---

# Transaction

You can use this query to learn more about the **Transaction** type of the Sismo API.

```graphql
query {
  __type(name: "Transaction") {
    name
    kind
    description
    fields {
      name
      description
    }
  }
}
```

### Query stats for one transaction

Use this query to get stats for a specific transaction hash by it's `id` and `network`.

{% tabs %}
{% tab title="Query" %}
```graphql
query {
  transaction(
    network: gnosis,
    id: "0x00031ef7e167cecd4f3429c5e50d664eb54ebf46eef752662c82684fa60545d2"
  ) {
    id
    from
    gasPrice
    gasUsed
    gasFee
    timestamp
  }
}
```


{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "transaction": {
      "id": "0x00031ef7e167cecd4f3429c5e50d664eb54ebf46eef752662c82684fa60545d2",
      "from": "0x49501a1e2b8442ecdd392c45cdd56ee256823f9e",
      "gasPrice": "3231725671",
      "gasUsed": "631043",
      "gasFee": "2039357862604853",
      "timestamp": "1673765315"
    }
  }
}
```
{% endtab %}

{% tab title="Schema" %}
```graphql
type Query {
  transaction(
    network: Network!,
    id: ID!
  ): Transaction
}
```
{% endtab %}
{% endtabs %}

### Query stats for a list of transactions

Use this query to get stats for a list of transactions. You can use [common parameters](common-parameters.md#parameters-when-querying-a-list) to filters them. The tab Schema below provides more details.

{% tabs %}
{% tab title="Query" %}
```graphql
query  {
  transactions(
    first: 5,
    orderBy: timestamp,
    orderDirection: desc,
    where: { network: polygon }
  ) {
    id
    from
    gasPrice
    gasUsed
    gasFee
    timestamp
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "transactions": [
      {
        "id": "0x35e76e4101078a7025a6b7018647b071d172ea6bf36adcd859f7436152fa4ddb",
        "from": "0x61126d5372f7f5627bb612e2f503787196587643",
        "gasPrice": "96900450582",
        "gasUsed": "631067",
        "gasFee": "61150676647430994",
        "timestamp": "1674658014"
      },
      {
        "id": "0x4387bbdc6a343e0dc5334617cd62d82ab1b1dc8069e0ce831866e885d75614e5",
        "from": "0x824a071fe74f62ec0fd6e64887f8e81f7280dbb5",
        "gasPrice": "157682127300",
        "gasUsed": "631079",
        "gasFee": "99509879214356700",
        "timestamp": "1674657792"
      },
      {
        "id": "0x4820193a10c18d13ea14def848a79598e7468a657ad3bd20d860bb3e19278065",
        "from": "0xbdb4ee6a96839a62e3a1a0db71bbc17c06326484",
        "gasPrice": "166412164279",
        "gasUsed": "631067",
        "gasFee": "105017225275055693",
        "timestamp": "1674657782"
      },
      {
        "id": "0x44d803e21f5e693bd52e7cce59cc4b7d824658546c001b267f62444146473f4d",
        "from": "0x61126d5372f7f5627bb612e2f503787196587643",
        "gasPrice": "168938994450",
        "gasUsed": "631067",
        "gasFee": "106611824410578150",
        "timestamp": "1674657780"
      },
      {
        "id": "0x35e614f8409b22a7af19280c48a71f02fbd576e74461a7c372526cf8e5dca911",
        "from": "0x97d0c8b93bff10a21c2d3468eb9b6e9265133eea",
        "gasPrice": "192332061842",
        "gasUsed": "631055",
        "gasFee": "121372109285703310",
        "timestamp": "1674657690"
      }
    ]
  }
}
```
{% endtab %}

{% tab title="Schema" %}
```graphql
enum Transaction_orderBy {
  id
  from
  timestamp
  gasPrice
  gasUsed
  gasFee
}

input Transaction_filter {
  id: String
  id_not: String
  id_in: [String]
  id_not_in: [String]

  network: Network 
  network_not: Network
  network_in: [Network]
  network_not_in: [Network]
  
  from: String
  from_not: String
  from_in: [String]
  from_not_in: [String]

  timestamp: DateTime
  timestamp_gt: DateTime
  timestamp_lt: DateTime
  timestamp_not: DateTime
  timestamp_in: [DateTime]
  timestamp_not_in: [DateTime]

  gasPrice: BigInt
  gasPrice_gt: BigInt
  gasPrice_lt: BigInt
  gasPrice_not: BigInt
  gasPrice_in: [BigInt]
  gasPrice_not_in: [BigInt]

  gasUsed: BigInt
  gasUsed_gt: BigInt
  gasUsed_lt: BigInt
  gasUsed_not: BigInt
  gasUsed_in: [BigInt]
  gasUsed_not_in: [BigInt]

  gasFee: BigInt
  gasFee_gt: BigInt
  gasFee_lt: BigInt
  gasFee_not: BigInt
  gasFee_in: [BigInt]
  gasFee_not_in: [BigInt]
}

type Query {
  transactions(
    where: Transaction_filter,
    first: Int,
    skip: Int,
    orderBy: Transaction_orderBy,
    orderDirection: Direction
  ): [Transaction]
}
```
{% endtab %}
{% endtabs %}

#### Examples

_Get the 10 most expensive transaction on gnosis_

{% tabs %}
{% tab title="Query" %}
```graphql
query  {
  transactions(
    first: 10,
    orderBy: gasFee,
    orderDirection: desc,
    where: { network: gnosis }
  ) {
    id
    from
    gasPrice
    gasUsed
    timestamp
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "transactions": [
      {
        "id": "0x81cd3347cb7723c25b24d385a07948eae26c8f933fa3417f6fa0a7a3140a6dfc",
        "from": "0x49501a1e2b8442ecdd392c45cdd56ee256823f9e",
        "gasPrice": "694773375241",
        "gasUsed": "631055",
        "timestamp": "1673427205"
      },
      {
        "id": "0xb29abcb1d654090825acfd6e4471af98e385ce560496e541cacb001f52e33570",
        "from": "0x49501a1e2b8442ecdd392c45cdd56ee256823f9e",
        "gasPrice": "694773375241",
        "gasUsed": "631031",
        "timestamp": "1673427225"
      },
      {
        "id": "0xa962d75638ced74db73ca968932766e6846281f9da542251e201470fdae93a5b",
        "from": "0x49501a1e2b8442ecdd392c45cdd56ee256823f9e",
        "gasPrice": "694773375241",
        "gasUsed": "631031",
        "timestamp": "1673427220"
      },
      {
        "id": "0x5a9fe7ad7cef0d30d9b0855d7909ba11ae65d74c1f7f066314e711368932dcef",
        "from": "0x49501a1e2b8442ecdd392c45cdd56ee256823f9e",
        "gasPrice": "694773375241",
        "gasUsed": "631031",
        "timestamp": "1673427225"
      },
      {
        "id": "0xb9565d7f1614a2cd583771e04639d88ac2d4b6cc5f5fd8946b96e7b06b085794",
        "from": "0x49501a1e2b8442ecdd392c45cdd56ee256823f9e",
        "gasPrice": "618902797055",
        "gasUsed": "631079",
        "timestamp": "1673381450"
      },
      {
        "id": "0x1de047af4c7a0ccced74d43901610491db1bd6919748fb1f7c294fceb5214673",
        "from": "0x49501a1e2b8442ecdd392c45cdd56ee256823f9e",
        "gasPrice": "618902797055",
        "gasUsed": "631043",
        "timestamp": "1673381455"
      },
      {
        "id": "0x3ee0a64c80b5f3b2ff134bb2fe2ac7c13afc862fa16d03742abe1c749e26c031",
        "from": "0x49501a1e2b8442ecdd392c45cdd56ee256823f9e",
        "gasPrice": "618902797055",
        "gasUsed": "631019",
        "timestamp": "1673381465"
      },
      {
        "id": "0x240c5b37ae96c71f5bcec3141d830aeb8fda1a16efbf1c2cd8bc7a33d41388eb",
        "from": "0x49501a1e2b8442ecdd392c45cdd56ee256823f9e",
        "gasPrice": "553201105536",
        "gasUsed": "631067",
        "timestamp": "1673418520"
      },
      {
        "id": "0xd2573a262611176e800b680e4185f52e56ac4ed8d0c97beca22059e199439d20",
        "from": "0x49501a1e2b8442ecdd392c45cdd56ee256823f9e",
        "gasPrice": "553201105536",
        "gasUsed": "631043",
        "timestamp": "1673418480"
      },
      {
        "id": "0xad4d84193b54e4ffcca42ef9040121af8583b647d2d578d8829eba670b003d3f",
        "from": "0x49501a1e2b8442ecdd392c45cdd56ee256823f9e",
        "gasPrice": "553201105536",
        "gasUsed": "631019",
        "timestamp": "1673418490"
      }
    ]
  }
}
```
{% endtab %}
{% endtabs %}
