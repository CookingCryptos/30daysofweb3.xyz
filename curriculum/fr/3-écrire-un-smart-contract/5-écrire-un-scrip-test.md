---
title: Écrire un script de test
description: Test your Solidity smart contract using the Hardhat.
optional: false
tweet: "Learn to test smart contracts with #30DaysofWeb3 @womenbuildweb3 ✍️"
---

Nous pouvons tester notre smart contract localement avant de le déployer sur un testnet afin de nous assurer qu'il fonctionne comme prévu. Un testnet (réseau de test) est un réseau blockchain alternatif utilisé pour exécuter et tester des programmes avant de diffuser des mises à jour sur le mainnet (réseau principal).

Créez un fichier dans le dossier `scripts` appelé `run.js`. Nous allons commencer par importer Hardhat en haut du fichier pour pouvoir l'utiliser plus tard.

Créez une fonction asynchrone(async) appelée `main` et une autre fonction asynchrone(async) appelée `runMain`, qui utilisera une instruction `try...catch` pour exécuter la fonction `main`. Au bas du fichier, nous pouvons exécuter `runMain()`.

```javascript
const hre = require("hardhat");

const main = async () => {};

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

Dans notre fonction `main`, nous pouvons utiliser hardhat pour déployer le contrat localement en ajoutant le code ci-dessous :

```javascript
const rsvpContractFactory = await hre.ethers.getContractFactory("Web3RSVP");
const rsvpContract = await rsvpContractFactory.deploy();
await rsvpContract.deployed();
console.log("Contract deployed to:", rsvpContract.address);
```

Hardhat nous permet d'accéder à différents wallets de test dans notre script afin de simuler différents wallets interagissant avec notre contrat. Pour obtenir l'adresse de notre Wallet de déploiement et quelques autres pour les tests, nous utilisons la méthode `getSigners`.

```javascript
const [deployer, address1, address2] = await hre.ethers.getSigners();
```

La première chose que nous voulons tester est la création d'un nouvel événement. Avant de pouvoir appeler cette méthode, nous devons définir les données de l'événement que nous allons utiliser. Vous pouvez utiliser un CID IPFS que nous avons déjà créé.

```javascript
let deposit = hre.ethers.utils.parseEther("1");
let maxCapacity = 3;
let timestamp = 1718926200;
let eventDataCID =
  "bafybeibhwfzx6oo5rymsxmkdxpmkfwyvbjrrwcl7cekmbzlupmp5ypkyfi";
```

_Extras_ : _Un horodatage est une séquence de caractères ou d'informations codées identifiant le moment où un certain événement s'est produit, donnant généralement la date et l'heure du jour, parfois précis à une petite fraction de seconde._ _Plus d'infos @ [unixtimestamp](https://www.unixtimestamp.com/)_

Ensuite, nous pouvons créer un nouvel événement avec nos données factices. Une fois la transaction terminée, `txn.wait` retournera des données sur la transaction, y compris un tableau des événements émis que nous pouvons enregistrer sur notre console. Nous pouvons sauvegarder l'identifiant de l'événement créé afin de pouvoir l'utiliser pour RSVP.

Vous pouvez loguer `wait` si vous voulez voir tout ce qui est retourné.

```javascript
let txn = await rsvpContract.createNewEvent(
  timestamp,
  deposit,
  maxCapacity,
  eventDataCID
);
let wait = await txn.wait();
console.log("NEW EVENT CREATED:", wait.events[0].event, wait.events[0].args);

let eventID = wait.events[0].args.eventID;
console.log("EVENT ID:", eventID);
```

Nous pouvons avoir chaque compte que nous avons obtenu de `getSigners` RSVP à l'événement. Par défaut, Hardhat appellera nos fonctions de contrat à partir de l'adresse du Wallet du déployeur. Pour appeler une fonction de contrat depuis un autre Wallet, nous pouvons utiliser le modificateur `.connect(address)`.

Pour envoyer notre dépôt, nous pouvons passer un objet comme dernier paramètre avec la valeur fixée au montant du dépôt.

```javascript
txn = await rsvpContract.createNewRSVP(eventID, { value: deposit });
wait = await txn.wait();
console.log("NEW RSVP:", wait.events[0].event, wait.events[0].args);

txn = await rsvpContract
  .connect(address1)
  .createNewRSVP(eventID, { value: deposit });
wait = await txn.wait();
console.log("NEW RSVP:", wait.events[0].event, wait.events[0].args);

txn = await rsvpContract
  .connect(address2)
  .createNewRSVP(eventID, { value: deposit });
wait = await txn.wait();
console.log("NEW RSVP:", wait.events[0].event, wait.events[0].args);
```

Nous pouvons confirmer tous les RSVP avec `confirmAllAttendees`. Puisque nous avons créé l'événement à partir de l'adresse du déployeur, nous devons appeler cette fonction à partir de l'adresse du déployeur également.

```javascript
txn = await rsvpContract.confirmAllAttendees(eventID);
wait = await txn.wait();
wait.events.forEach((event) =>
  console.log("CONFIRMED:", event.args.attendeeAddress)
);
```

Parce que nous exigeons que le propriétaire de l'événement attende 7 jours avant de retirer les dépôts non réclamés, il échouera si nous essayons d'appeler cette fonction maintenant.

Pour contourner ce problème, hardhat nous permet de simuler le passage du temps. Nous pouvons attendre 10 ans pour nous assurer qu'il y a eu suffisamment de temps.

```javascript
// wait 10 years
await hre.network.provider.send("evm_increaseTime", [15778800000000]);

txn = await rsvpContract.withdrawUnclaimedDeposits(eventID);
wait = await txn.wait();
console.log("WITHDRAWN:", wait.events[0].event, wait.events[0].args);
```

Pour tester facilement ce script, nous pouvons ajouter un raccourci dans notre fichier `package.json`.

```json
"scripts": {
    "script": "node scripts/run.js"
```

Avant de pouvoir tester le script, nous devons exécuter `npx hardhat compile` dans le terminal pour compiler notre contrat.

Une fois que c'est fait, allez-y et testez le script avec `npm run script` pour vous assurer que votre contrat fonctionne comme prévu.

Maintenant, nous pouvons exécuter `npm run script` dans notre terminal quand nous voulons tester notre contrat.

---

Écrivains: [Sarah Schwartz](https://twitter.com/schwartzswartz),
Rédacteurs: [Sarah Z](https://twitter.com/haegeez)
Traducteur: [CookingCryptos](https://twitter.com/CookingCryptos)

