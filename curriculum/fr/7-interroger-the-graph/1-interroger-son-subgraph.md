---
title: Interroger votre subgraph
description: Setup Apollo GraphQL client to query your subgraph.
optional: false
tweet: "Query a subgraph for a full-stack dapp with #30DaysofWeb3 @womenbuildweb3 ⛓"
---

Pour interroger facilement notre subgraph depuis notre frontend, nous allons utiliser le client Apollo GraphQL.

Dans le répertoire racine de notre frontend, nous pouvons ajouter un fichier appelé `apollo-client.js` et ajouter le code ci-dessous avec l'url de votre subgraph déployé :

```javascript
import { ApolloClient, InMemoryCache } from "@apollo/client";

const client = new ApolloClient({
  uri: "https://api.thegraph.com/subgraphs/name/[YOUR_GITHUB]/[YOUR_SUBGRAPH]",
  cache: new InMemoryCache(),
});

export default client;
```

Dans notre fichier `_app.js`, nous pouvons importer le fournisseur et le client Apollo en haut du fichier, et intégrer notre composant `Layout` dans le fournisseur Apollo.

```javascript
import { ApolloProvider } from "@apollo/client";
import client from "../apollo-client";
```

```javascript
<ApolloProvider client={client}>
  <Layout>
    <Component {...pageProps} />
  </Layout>
</ApolloProvider>
```

Si vous rencontrez des erreurs en intégrant le composant Layout dans le ApolloProvider, voici la fonction `MyApp` complète pour référence :

```javascript
export default function MyApp({ Component, pageProps }) {
  return (
    <WagmiConfig client={wagmiClient}>
      <RainbowKitProvider chains={chains}>
        <ApolloProvider client={client}>
          <Layout>
            <Component {...pageProps} />
          </Layout>
        </ApolloProvider>
      </RainbowKitProvider>
    </WagmiConfig>
  );
}
```

Maintenant nous pouvons facilement accéder au client apollo dans chacune de nos pages où nous voulons interroger notre subgraph !

## ✋ Need Help?

If you need help, check to see if your question has already been asked in **#section-7-help**. If you don't see it in there, post a question with any details that would make it easy for a team member to help you. We'll answer most frequently asked questions in live office hours, so keep an eye out in **#announcements** for those!

---

Écrivains: [Sarah Schwartz](https://twitter.com/schwartzswartz),
Rédacteurs: [Sarah Z](https://twitter.com/haegeez)
Traducteur: [CookingCryptos](https://twitter.com/CookingCryptos)

