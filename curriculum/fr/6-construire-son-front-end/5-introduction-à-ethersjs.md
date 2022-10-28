---
title: Se connecter à notre contrat avec Ethers.js
description: Get started with Ethers.js, a JavaScript library that enables you to interact with Ethereum.
optional: false
tweet: "Connect to a smart contract using Ethers.js with #30DaysofWeb3 @womenbuildweb3 💥"
---

## Qu'est-ce qu'Ethers.js ?

**Ethers.js** est _une bibliothèque JavaScript permettant aux développeurs d'interagir facilement avec la blockchain Ethereum et son écosystème.

Les applications du conteneur Wallet d'Ethers vivent à l'intérieur d'une iframe qui les protège les unes des autres et des données privées (comme les clés privées).

Pour les opérations en lecture seule, l'application se connecte directement à la blockchain Ethereum.

Pour écrire sur la blockchain, la dApp transmet les messages et les transactions au conteneur et abandonne le contrôle de l'application. Une fois que l'utilisateur a approuvé (ou refusé) la transaction, le contrôle revient à la dApp et une copie signée du message ou de la transaction est renvoyée.

La bibliothèque d'applications Ethers gère toutes ces interactions pour vous.

## Connexion à notre contrat

Parce que nous voulons nous connecter à notre contrat sur plusieurs pages différentes, nous allons ajouter le code de notre front-end pour communiquer avec notre Smart Contract dans le dossier `utils`.

Dans le dossier `utils`, créez les deux fichiers suivants :

- Notre premier fichier sera appelé `connectContract.js`.
- Pour notre deuxième fichier, nous le nommerons `Web3RSVP.json` .

`Web3RSVP.json` est le fichier qui va gérer le stockage de l'ABI du contrat pour permettre à notre front-end de communiquer avec notre contrat. Ouvrez le dossier du projet pour notre smart contract, copiez l'ABI du dossier `artifacts/contracts`, et collez-le dans `Web3RSVP.json`.

En haut de `connectContract.js`, nous pouvons importer ethers et notre ABI.

```javascript
import abiJSON from "./Web3RSVP.json";
import { ethers } from "ethers";
```

En dessous, nous allons créer une fonction appelée `connectContract`. Assurez-vous d'exporter la fonction en bas du fichier.

Nous avons accès à l'API globale d'Ethereum, qui est accessible via l'objet `window` dans `window.ethereum`. Nous avons besoin d'accéder à cet objet afin de nous connecter à notre contrat, donc nous allons envelopper notre logique dans une déclaration `try..catch` afin que nous puissions facilement attraper les erreurs.

À la fin de la fonction, nous voulons retourner le contrat afin de pouvoir l'appeler depuis un autre composant. Assurez-vous de remplacer "[YOUR_CONTRACT_ADDRESS]" par l'adresse du contrat que vous avez déployé.

**Note:** : si vous n'avez pas noté l'adresse de votre contrat lors des leçons précédentes, vous pouvez retourner sur le site [Mumbai PolygonScan](https://mumbai.polygonscan.com/) et coller l'adresse de votre Wallet Coinbase. Là, vous devriez voir une transaction qui dit quelque chose comme `Create : Web3RSVP " ou le nom que vous avez donné à votre contrat. Cliquez sur ce lien pour être dirigé vers la page. En haut, vous verrez l'adresse de votre contrat. Copiez-la et collez-la dans le champ approprié de la constante contractAddress ci-dessous :

```javascript
function connectContract() {
   //Note : Votre contractAddress commencera par 0x, supprimez tout ce qui se trouve entre les guillemets et collez votre adresse de contrat.
  const contractAddress = "0x[YOUR_CONTRACT_ADDRESS]";
  const contractABI = abiJSON.abi;
  let rsvpContract;
  try {
    const { ethereum } = window;

    if (ethereum) {
      //vérification de l'objet eth dans la fenêtre
      const provider = new ethers.providers.Web3Provider(ethereum);
      const signer = provider.getSigner();
      rsvpContract = new ethers.Contract(contractAddress, contractABI, signer); // création d'une nouvelle connexion au contrat
    } else {
      console.log("Ethereum object doesn't exist!");
    }
  } catch (error) {
    console.log("ERROR:", error);
  }
  return rsvpContract;
}

export default connectContract;
```

Maintenant que nous pouvons nous connecter à notre contrat, nous pouvons appeler une fonction pour créer un nouvel événement dans la section suivante.

## ✋ Need Help?

If you need help, check to see if your question has already been asked in **#section-6-help**. If you don't see it in there, post a question with any details that would make it easy for a team member to help you. We'll answer most frequently asked questions in live office hours, so keep an eye out in **#announcements** for those!

## 🏝 C'est l'heure de la pause

C'est le bon moment pour faire une pause. Mettez à jour le Twitterverse en appuyant sur le bouton Partager ci-dessous, collez votre tweet dans **#builders-hype** et faites du bruit pour les autres constructeurs 🔥.

**Fin du 10ème jour**

---

Écrivains: [Avni Agrawal](https://twitter.com/AvniAgrawal1802),
Rédacteurs: [Sarah Schwartz](https://twitter.com/schwartzswartz)
Traducteur: [CookingCryptos](https://twitter.com/CookingCryptos)
