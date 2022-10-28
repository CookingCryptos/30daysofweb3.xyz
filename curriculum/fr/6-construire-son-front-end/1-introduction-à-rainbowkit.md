---
title: Connecter le wallet avec RainbowKit
description: Easily support intuitive wallet connection experiences in your dapp by using RainbowKit, a React library.
optional: false
tweet: "Learn to use @rainbowdotme's RainbowKit with #30DaysofWeb3 @womenbuildweb3 🌈"
---

![RainbowKit UI](https://i.imgur.com/QgE9oIj.jpg)

## Qu'est-ce que RainbowKit ?

**RainbowKit** est _une bibliothèque React qui permet aux développeurs d'ajouter facilement une connexion Wallet à leur dapp_. Elle est simple à utiliser, réactive, personnalisable et adaptable. Des fonctionnalités de base comme la connexion et la déconnexion d'un Wallet, à l'affichage des soldes, RainbowKit est capable de travailler avec différents Wallets, de changer de chaînes et de convertir des adresses en ENS (Ethereum Name Service).

Vous pouvez entièrement personnaliser votre thème RainbowKit et n'inclure que les fonctionnalités nécessaires à vos dApps. RainbowKit utilise les bibliothèques les plus couramment utilisées dans l'écosystème web3 : ethers et wagmi.

## Chargement des variables d'environnement

A la racine de votre projet, créez un nouveau fichier appelé `.env.local`. Vous pouvez utiliser la commande `touch .env.local` dans votre terminal pour créer ce fichier. Dans le web3rsvp-frontend-starter, il y a un fichier appelé **.env.example** qui montre un exemple de la façon de configurer votre fichier .env.local. Ce fichier est l'endroit où nous garderons des informations secrètes comme nos clés API afin qu'elles ne soient pas exposées sur le frontend. 

Le fichier `.env.local` devrait ressembler à quelque chose comme ceci :

```

# Créez un fichier .env.local et remplissez-le avec les valeurs correspondantes.

// C'est ici que vous placez le token de l'API Web3storage.
WEB3STORAGE_TOKEN=<Api_Token>

// C'est ici que se trouve la clé API Alchemy ou Infura.
NEXT_PUBLIC_INFURA_ID=<Your Infura API key>
NEXT_PUBLIC_TESTNET_EXPLORER_URL=https://mumbai.polygonscan.com/
```

Pendant que vous êtes dans ce fichier, vous pouvez aussi remplacer `<Your Infura API key>` par votre clé API Infura. Vous pouvez la trouver en allant sur votre [Infura dashboard] (https://infura.io/dashboard) et en sélectionnant `MANAGE KEY`. Si vous utilisez Alchemy, vous pouvez trouver la clé API dans votre [Alchemy dashboard](https://dashboard.alchemyapi.io/) et en sélectionnant `VIEW KEY`.

> Remarque : ne vous préoccupez pas du token API de Web3storage. Nous allons configurer Webstorage dans une section ultérieure.

> Remarque : Infura a renommé project id en API Key. [see more here](https://docs.infura.io/infura/networks/ethereum/how-to/secure-a-project/project-id) - vous ne verrez plus project id dans vos paramètres de projet. Sélectionnez la clé API qui se trouve sous l'onglet endpoints de vos paramètres de projet !

## Importation et configuration des chaînes

Nous pouvons configurer RainbowKit dans notre fichier `_app.js` (situé dans le dossier `pages` du projet). Pour configurer les chaînes, ainsi que les connecteurs qui sont nécessaires, un client wagmi doit être mis en place. Vous êtes libre d'utiliser autant de chaînes que vous le souhaitez mais dans notre dApp, nous avons utilisé la chaîne Polygon puisque nous avons déployé sur le testnet Polygon (Mumbai).

Nous allons commencer par ajouter les imports suivants en haut de notre fichier, en dessous de la ligne `import "../styles/globals.css";` :

```javascript
import "@rainbow-me/rainbowkit/styles.css";
import { getDefaultWallets, RainbowKitProvider } from "@rainbow-me/rainbowkit";
import { chain, configureChains, createClient, WagmiConfig } from "wagmi";
import { infuraProvider } from "wagmi/providers/infura";
import { publicProvider } from "wagmi/providers/public";
```

Ensuite, nous devrons configurer les chaînes auxquelles nous voulons nous connecter avec notre project ID d'Infura et initialiser le `wagmiClient`.

```javascript
const infuraId = process.env.NEXT_PUBLIC_INFURA_ID;

const { chains, provider } = configureChains(
  [chain.polygon],
  [infuraProvider({ infuraId }), publicProvider()]
);

const { connectors } = getDefaultWallets({
  appName: "web3rsvp",
  chains,
});

const wagmiClient = createClient({
  autoConnect: true,
  connectors,
  provider,
});
```

En définissant `autoConnect` à `true`, nous pouvons garder l'utilisateur connecté automatiquement afin qu'il n'ait à connecter son Wallet qu'une seule fois.

Dans notre fichier `_app.js`, nous pouvons envelopper notre application avec `RainbowKitProvider` et `WagmiConfig`.

```javascript
export default function MyApp({ Component, pageProps }) {
  return (
    <WagmiConfig client={wagmiClient}>
      <RainbowKitProvider chains={chains}>
        <Layout>
          <Component {...pageProps} />
        </Layout>
      </RainbowKitProvider>
    </WagmiConfig>
  );
}
```

## ✋ Need Help?

If you need help, check to see if your question has already been asked in **#section-6-help**. If you don't see it in there, post a question with any details that would make it easy for a team member to help you. We'll answer most frequently asked questions in live office hours, so keep an eye out in **#announcements** for those!

---

Écrivains: [Busayo](https://twitter.com/AmoweO),
Rédacteurs: [Sarah Z](https://twitter.com/haegeez), [Krystal](https://twitter.com/theekrystallee),
Traducteur: [CookingCryptos](https://twitter.com/CookingCryptos)
