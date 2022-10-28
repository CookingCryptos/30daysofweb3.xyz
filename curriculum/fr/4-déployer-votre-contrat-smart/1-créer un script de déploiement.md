---
title: Création de votre script de déploiement
description: Deploy your smart contract to Polygon Mumbai test network using Hardhat.
optional: false
tweet: "Deploy a smart contract to @0xPolygon Mumbai testnet with #30DaysofWeb3 @womenbuildweb3 💪"
---

Dans notre dossier de scripts, nous pouvons créer un fichier appelé `deploy.js`, où nous pouvons coller une copie de notre script de test et enlever toutes les transactions de test. Notre fichier `deploy.js` devrait ressembler à ceci :

```javascript
const hre = require("hardhat");

const main = async () => {
  const rsvpContractFactory = await hre.ethers.getContractFactory("Web3RSVP");
  const rsvpContract = await rsvpContractFactory.deploy();
  await rsvpContract.deployed();
  console.log("Contract deployed to:", rsvpContract.address);
};

const runMain = async () => {
  try {
    await main();
    process.exit(0);
  } catch (error) {
    console.log(error);
    process.exit(1);
  }
};

runMain();
```

## Déploiement sur le réseau de test de Mumbai

Nous aurons besoin de token de test MATIC dans notre Wallet Coinbase sur le réseau de Mumbai afin de déployer notre contrat.

Pour cette étape, vous aurez besoin de l'adresse de votre Wallet. Pour la trouver dans votre Coinbase Wallet, cliquez sur l'onglet "Assets" (le premier onglet de l'application). Ensuite, cliquez sur la flèche orientée vers le bas pour afficher l'adresse de votre Wallet. Vous devez copier celle qui indique "Votre adresse Ethereum". Après avoir copié votre adresse, obtenez des tokens de test gratuit MATIC en entrant l'adresse de votre Wallet : https://faucet.polygon.technology/.

Avant de déployer notre contrat, nous voulons nous assurer que nous avons compilé la dernière version. Pour ce faire, nous pouvons exécuter `npx hardhat compile` dans le terminal depuis le dossier du projet. En cas de succès, vous devriez voir un message qui dit : "Compilé 1 fichier Solidity avec succès".

Pour déployer, tout ce que nous devons faire est d'exécuter le script de déploiement que nous venons de créer. Nous pouvons ajouter un script dans notre fichier `package.json` afin de pouvoir réutiliser notre commande de déploiement si jamais nous en avons besoin à nouveau.

```json
"scripts" : {
    "deploy" : "npx hardhat run scripts/deploy.js --network mumbai",

```

Si jamais nous devons changer quelque chose dans notre contrat et redéployer, tout ce que nous avons à faire est de lancer `npx hardhat compile` et ensuite `npm run deploy`. Si cette opération a réussi, vous verrez quelque chose de similaire à ce message dans votre terminal : `Contrat déployé à : 0x80B72dcab8FA1d220E9e8bc93acC75EA36535623`.


## ✋ Need Help?

If you need help, check to see if your question has already been asked in **#section-4-help**. If you don't see it in there, post a question with any details that would make it easy for a team member to help you. We'll answer most frequently asked questions in live office hours, so keep an eye out in **#announcements** for those!

---

Écrivains: [Sarah Schwartz](https://twitter.com/schwartzswartz),
Rédacteurs: [Kristen](https://twitter.com/cuddleofdeath)
Traducteur: [CookingCryptos](https://twitter.com/CookingCryptos)

