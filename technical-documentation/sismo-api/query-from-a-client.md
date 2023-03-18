# Query from a client

There are several ways to query our GraphQL API :

{% embed url="https://api.sismo.io/" %}
Visit the Explorer and interact with our API
{% endembed %}

<details>

<summary>GraphQL Client</summary>

You can use any GraphQL client. Here is an example with Apollo Client:

```typescript
import { ApolloClient, InMemoryCache, gql } from '@apollo/client';

const client = new ApolloClient({
  uri: 'https://api.sismo.io',
  cache: new InMemoryCache(),
});
const query = gql`
query {
  mintedBadges {
    balance
    tokenId
    badge {
      name
    }
  }
}`
const result = await client.query({ query });
```

</details>

<details>

<summary>POST Request</summary>

<pre class="language-typescript"><code class="lang-typescript"><strong>const query = `
</strong>query {
  mintedBadges {
    level
    badge {
      tokenId
    }
    badge {
      name
    }
  }
}`

<strong>const res = await fetch('https://api.sismo.io', {
</strong>  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'Accept': 'application/json',
  },
  body: JSON.stringify({ query})
});
const { data, errors } = await res.json();
</code></pre>



</details>

<details>

<summary>cURL</summary>

```bash
curl -g \
-X POST https://api.sismo.io/ \
-H "Content-Type: application/json" \
--data-binary @- << EOF
{
  "query": "query { mintedBadges { level badge { tokenId } badge { name } } }"
}
EOF

```

</details>
