---
title: Appeler votre contrat
description: Call your smart contract methods using Ethers.js.
optional: false
tweet: "Call smart contract methods using Ethers.js with #30DaysofWeb3 @womenbuildweb3 💥"
---

Ouvrez le fichier `create-event.js` dans le dossier `pages`. Vous pouvez voir un aperçu de cette page en allant sur http://localhost:3000/create-event. Vous devriez voir un formulaire avec tous les champs de saisie dont nous avons besoin déjà mis en place.

**Note:** Si vous ne voyez rien, assurez-vous d'avoir exécuté `npm run dev` dans votre terminal à l'intérieur du dossier de votre projet.

En cliquant sur le bouton de création, la méthode `handleSubmit` sera appelée. Pour l'instant, la console enregistre juste "Form submitted". Maintenant, nous allons passer en revue et mettre en place la logique qui doit se produire lorsque le formulaire est soumis.

Nous utilisons des variables d'état pour garder la trace des données du formulaire. Nous allons les organiser en un seul objet que nous utiliserons pour passer à notre endpoint API qui stockera certaines de nos données d'événement off-chain via Web3.Storage.

Ajoutez ceci à l'intérieur de votre fonction handleSubmit, juste sous `e.preventDefault()` :

```javascript
const body = {
  name: eventName,
  description: eventDescription,
  link: eventLink,
  image: getRandomImage(),
};
```

Pour l'image, nous allons importer les deux éléments suivants en haut du fichier `create-event.js` :

- Notre première importation va tirer la fonction `getRandomImage` de notre fichier `getRandomImage.js`.
- Ensuite, nous allons également importer `ethers` afin de pouvoir l'utiliser pour appeler notre contrat.

Vos imports ressembleront à ceci :

```javascript
import getRandomImage from "../utils/getRandomImage";
import { ethers } from "ethers";
```

Vous remarquerez que nous n'envoyons pas toutes les données de l'événement ici. En effet, le dépôt de l'événement, la capacité maximale, l'horodatage, etc. seront stockés dans la chaîne avec notre contrat intelligent. Avant de pouvoir appeler notre contrat, nous devons stocker des données d'événement sur Web3.Storage et obtenir notre CID.

Dans notre fonction `handleSubmit`, nous pouvons utiliser une instruction `try..catch` pour envoyer le corps à notre endpoint API /store-event-data. Si nous obtenons une réponse positive, c'est-à-dire que nous avons pu stocker les données avec Web3.Storage et que nous avons obtenu un CID, nous pouvons le passer dans une nouvelle fonction appelée `createEvent`. Voici à quoi devrait ressembler votre fonction :

```javascript
async function handleSubmit(e) {
  e.preventDefault();

  const body = {
    name: eventName,
    description: eventDescription,
    link: eventLink,
    image: getRandomImage(),
  };

  try {
    const response = await fetch("/api/store-event-data", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify(body),
    });
    if (response.status !== 200) {
      alert("Oops! Something went wrong. Please refresh and try again.");
    } else {
      console.log("Form successfully submitted!");
      let responseJSON = await response.json();
      await createEvent(responseJSON.cid);
    }
    // vérifier la réponse, si le succès est faux, ne pas les amener à la page de succès
  } catch (error) {
    alert(
      `Oops! Something went wrong. Please refresh and try again. Error ${error}`
    );
  }
}
```

Pour connecter notre contrat, nous allons importer la fonction que nous avons écrite plus tôt depuis le dossier `utils` comme ceci :

```javascript
import connectContract from "../utils/connectContract";
```

Ensuite, nous allons créer une nouvelle fonction appelée `createEvent` où nous pourrons passer les données de l'événement dans la fonction `createNewEvent` de notre contrat.

Nous devrons nous assurer que nous envoyons le montant du dépôt de l'événement en **wei**, qui est _la plus petite dénomination d'ETH_ (1 ETH = 1000000000000000000 Wei). Nous pouvons utiliser une méthode d'ethers appelée `parseEther` pour facilement analyser un montant en ETH (ou MATIC dans ce cas) au montant correct que notre contrat peut comprendre.

Nous devons également générer un timestamp unix à partir des entrées de date et d'heure de notre formulaire.

Pour appeler notre contrat, nous pouvons simplement appeler la méthode comme ceci (**Note:** La fonction await ci-dessous est simplement un exemple) :

```javascript
await contract.methodName(parameters, { optionName: optionValue });
```

Après avoir transmis les paramètres de la fonction, nous pouvons également transmettre un objet dans lequel nous pouvons définir la limite de gas pour la transaction.

Cette fonction retournera un objet transaction avec plus de données sur notre transaction. Pour accéder facilement à ces informations, comme le hash de la transaction, nous pouvons les stocker dans une variable appelée `txn`.

```javascript
const createEvent = async (cid) => {
  try {
    const rsvpContract = connectContract();

    if (rsvpContract) {
      let deposit = ethers.utils.parseEther(refund);
      let eventDateAndTime = new Date(`${eventDate} ${eventTime}`);
      let eventTimestamp = eventDateAndTime.getTime();
      let eventDataCID = cid;

      const txn = await rsvpContract.createNewEvent(
        eventTimestamp,
        deposit,
        maxCapacity,
        eventDataCID,
        { gasLimit: 900000 }
      );
      console.log("Minting...", txn.hash);
      console.log("Minted -- ", txn.hash);
    } else {
      console.log("Error getting contract.");
    }
  } catch (error) {
    console.log(error);
  }
};
```

---

Écrivains: [Sarah Schwartz](https://twitter.com/schwartzswartz),
Rédacteurs: [Sarah Z](https://twitter.com/haegeez)
Traducteur: [CookingCryptos](https://twitter.com/CookingCryptos)
