---
title: Déployer avec Infura
description: Learn about Infura, a web3 infrastructure provider that helps developers to build apps on Ethereum without having to manage their own blockchain nodes.
optional: false
tweet: "Learn how @infura_io supports dapps built on Ethereum with #30DaysofWeb3 @womenbuildweb3 💪"
---

![Deploying With Infura](https://user-images.githubusercontent.com/15064710/180662065-a61d5bcb-a3fb-4b31-9b9a-37d1754095cf.png)

## Qu'est-ce qu'Infura ?

[**Infura**](https://infura.io/) est une société qui fournit des outils d'infrastructure web3 permettant aux développeurs de développer sur une blockchain. Infura fournit les nœuds qui sont le point d'entrée de la blockchain et vous permettent d'exécuter des transactions. Sans un fournisseur de nœuds comme Infura, un développeur devrait exécuter son propre nœud pour communiquer avec la blockchain, ce qui peut être coûteux, difficile et long.

## Comment Infura s'intègre dans votre projet

Une fois que nous avons déployé notre Smart Contract, nous avons besoin qu'il puisse communiquer avec la blockchain pour exécuter des transactions (création d'un nouvel événement, inscription à un événement existant, etc.) Comme nous ne voulons pas exécuter notre propre nœud, nous utiliserons l'infrastructure de nœuds d'Infura pour nous donner la fonctionnalité d'interagir avec la blockchain.

![Déployer avec le diagramme Infura](https://i.imgur.com/VlpZ6pr.png)

## Mise à jour de la configuration hardhat et installation de dotenv

Nous devons également mettre à jour notre fichier `hardhat.config.js` avec des informations sur le réseau sur lequel nous allons nous déployer, une URL de fournisseur d'Infura, et la clé privée de notre Wallet. Comme il s'agit d'informations sensibles ( Rappel : _NE JAMAIS PARTAGER VOTRE CLÉ PRIVÉE AVEC QUI QUE CE SOIT !_ ), nous allons stocker notre URL de fournisseur et notre clé privée dans un nouveau fichier .env à la racine de notre dossier.

À la racine de votre projet, créez un fichier appelé " .env ". Vous pouvez également exécuter `touch .env` dans votre terminal et il créera le fichier .env pour vous.

Ensuite, installez `dotenv` en exécutant `npm install dotenv`. Dotenv est un module qui charge les variables d'environnement à partir d'un fichier .env - il vous permettra de garder vos clés privées sécurisées tout en les utilisant dans votre application et vous évitera de les commit sur github.

Mettez à jour votre fichier `hardhat.config.js` comme ceci :

```javascript
require("@nomiclabs/hardhat-waffle"); //une nouvelle mise à jour de hardhat nécessite d'importer 'hardhat-toolbox'
require("dotenv").config();

module.exports = {
  solidity: "0.8.4",
  networks: {
    hardhat: {
      chainId: 1337,
    },
    mumbai: {
      url: process.env.STAGING_INFURA_URL,
      accounts: [`0x${process.env.STAGING_PRIVATE_KEY}`],
      gas: 2100000,
      gasPrice: 8000000000,
    },
  },
};
```

## Configurer Infura avec Polygon Mumbai

Naviguez sur https://infura.io/ et créez un compte.
Allez dans "Dashboard" et cliquez sur "Create new project". Sélectionnez 'Ethereum' dans la liste déroulante et nommez votre projet.

Dans la section "Keys", cliquez sur la liste déroulante pour changer le point de terminaison en Polygon Mumbai.
Si cette option est grisée, vous devrez la sélectionner en tant que module complémentaire. Infura peut demander une carte de crédit pour protéger le service contre le spamming par des bots, mais ne vous facturera pas (assurez-vous que le montant total indique 0 $).

Copiez le nouveau point de terminaison qui apparaît dans Infura après avoir sélectionné Polygon Mumbai et enregistrez-le comme variable de votre projet dans le fichier .env que nous venons de créer.

Ensuite, récupérez la clé privée de votre Wallet Coinbase en ouvrant l'extension et en allant dans _Settings > Advanced Settings > Show private key_. Connectez-vous à l'aide de votre mot de passe, puis copiez votre clé privée.

Retournez dans votre fichier .env et créez une variable appelée `STAGING_PRIVATE_KEY` et définissez la valeur comme étant votre clé privée que vous venez de copier. Voici à quoi devrait ressembler votre fichier `.env` maintenant

```
STAGING_INFURA_URL=https://polygon-mumbai.infura.io/v3/12345678
STAGING_PRIVATE_KEY=1234_YOUR_PRIVATE_KEY_5678
```

## ✋ Need Help?

If you need help, check to see if your question has already been asked in **#section-4-help**. If you don't see it in there, post a question with any details that would make it easy for a team member to help you. We'll answer most frequently asked questions in live office hours, so keep an eye out in **#announcements** for those!

---

Écrivains: [Sarah Schwartz](https://twitter.com/schwartzswartz),
Rédacteurs: [Kristen](https://twitter.com/cuddleofdeath)
Traducteur: [CookingCryptos](https://twitter.com/CookingCryptos)

