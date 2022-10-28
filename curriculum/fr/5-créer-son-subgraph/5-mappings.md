---
title: Mappings
description: Write the code that translates data from your datasources to the entities defined in your schema.
optional: false
tweet: "I just wrote mappings for my subgraph @graphprotocol with #30DaysofWeb3 @womenbuildweb3 👾"
---

Vous pouvez trouver le fichier des mappings (src/{Name}.ts) dans le dossier `src`. Les mappings utilisent AssemblyScript, un langage fortement typé basé sur Typescript, et agissent comme les résolveurs dans votre API GraphQL typique. Ils sont responsables de la logique qui se produit entre le déclenchement d'un event de notre smart contract et l'organisation de ces données dans notre schéma.

Ce fichier aura déjà un schéma de base généré pour vous. Vous devriez voir une fonction pour chaque gestionnaire d'event défini dans notre manifeste de subgraph. Chaque fonction dans notre mapping doit être exportée, et prend en argument l'event qu'elle va gérer.

Nous pouvons exécuter `graph codegen` dans le terminal pour générer des types AssemblyScript pour nos entités et events, et les importer en haut de notre fichier de mappings (assurez-vous que vous êtes dans le répertoire racine de votre dossier de projet et que vous avez sauvegardé vos changements avant d'exécuter cette commande). Avec cela, vous devriez également être en mesure de voir facilement toutes les propriétés de l'objet `event` dans votre éditeur de code.

![subgraph event object](https://i.imgur.com/gvYnfdX.png)

Si vous apportez d'autres modifications au schéma ou au manifeste du subgraph, vous pouvez toujours relancer `graph codegen` pour générer de nouveaux types.

Nous allons tout supprimer dans la première fonction et remplacer l'objet `Example Entity` importé en haut par les types d'entités que nous venons de générer. Nous n'aurons pas besoin non plus du type de contrat Web3RSVP importé dans la deuxième ligne puisque nous n'aurons pas besoin de faire d'appels au contrat. Nous pouvons également supprimer le type BigInt importé dans la première ligne, et le remplacer par les types `Address`, `ipfs`, et `json`.

Le fichier de mappings devrait maintenant ressembler à ceci :



```javascript
import { Address, ipfs, json } from "@graphprotocol/graph-ts";
import {
  ConfirmedAttendee,
  NewEventCreated,
  NewRSVP,
  DepositsPaidOut,
} from "../generated/Web3RSVP/Web3RSVP";
import { Account, RSVP, Confirmation, Event } from "../generated/schema";
import { integer } from "@protofire/subgraph-toolkit";

export function handleNewEventCreated(event: NewEventCreated): void {}

export function handleNewRSVP(event: NewRSVP): void {}

export function handleConfirmedAttendee(event: ConfirmedAttendee): void {}

export function handleDepositsPaidOut(event: DepositsPaidOut): void {}
```
Nous pouvons commencer par la fonction `handleNewEventCreated`. Nous pouvons créer une nouvelle instance d'une entité `Event` en passant un identifiant unique qui est une chaîne de caractères hexadécimaux.

Voici à quoi ressemblent nos events dans notre contrat :

```solidity
event NewEventCreated(
bytes32 eventID,
      address creatorAddress,
      uint256 eventTimestamp,
      uint256 maxCapacity,
      uint256 deposit,
      string eventDataCID
);

event NewRSVP(bytes32 eventID, address attendeeAddress);

event ConfirmedAttendee(bytes32 eventID, address attendeeAddress);

event DepositsPaidOut(bytes32 eventID);
```
Puisque nous avons déjà un `eventID` à partir de `NewEventCreated`, nous pouvons l'utiliser comme identifiant unique pour l'entité `Event`. Nous voulons également nous assurer que nous ne créons pas de doublons d'événements avec le même identifiant, donc nous pouvons essayer de charger cet événement en premier, et s'il n'est pas trouvé, nous pouvons le créer et le sauvegarder. (Note : Si vous oubliez d'utiliser la méthode `save` à la fin de votre fonction, ces données ne seront pas sauvegardées ! Assurez-vous toujours que vous sauvegardez les modifications apportées à une entité).

Comme le `eventID` émis par notre contrat est du type `Bytes32`, nous pouvons utiliser la méthode intégrée `toHex()` pour convertir l'id en une chaîne hexadécimale représentant les bytes du tableau.

```javascript
let newEvent = Event.load(event.params.eventID.toHex());
if (newEvent == null) {
  newEvent = new Event(event.params.eventID.toHex());
  newEvent.save();
}
```

Il s'agit d'un modèle standard que nous suivrons pour chacune de nos fonctions de mapping. Nous allons d'abord vérifier si nous pouvons charger notre entité avec un identifiant unique, et créer une nouvelle instance uniquement si ce résultat est nul.

La dernière chose que nous devons faire ici est de définir les valeurs de chaque champ de notre schéma. Nous pouvons accéder à la plupart de ces données dans l'objet event.params. Pour le champ `paidOut`, nous pouvons simplement le mettre à false.

```javascript
let newEvent = Event.load(event.params.eventID.toHex());
if (newEvent == null) {
newEvent = new Event(event.params.eventID.toHex());
newEvent.eventID = event.params.eventID;
newEvent.eventOwner = event.params.creatorAddress;
newEvent.eventTimestamp = event.params.eventTimestamp;
newEvent.maxCapacity = event.params.maxCapacity;
newEvent.deposit = event.params.deposit;
newEvent.paidOut = false;
```

Pour les champs `totalRSVP` et `totalConfirmedAttendees`, nous allons utiliser la boîte à outils de subgraphe protofire que nous avons ajoutée plus tôt. Dans notre fonction `handleNewEventCreated`, nous voulons mettre les totaux à 0 pour commencer, donc nous pouvons utiliser `integer.ZERO` pour mettre ces champs à 0.

```
newEvent.totalRSVP = integer.ZERO ;
newEvent.totalConfirmedAttendees = integer.ZERO ;
```

Pour les champs `name`, `description`, `link`, et `imagePath`, nous utiliserons le `eventCID` pour accéder aux données stockées avec ipfs (web3.storage). Nous pouvons utiliser le CID et le nom du fichier de détails de l'événement, `data.json`, pour extraire ces données.

```javascript
let metadata = ipfs.cat(event.params.eventDataCID + "/data.json");

if (metadata) {
  const value = json.fromBytes(metadata).toObject();
  if (value) {
    const name = value.get("name");
    const description = value.get("description");
    const link = value.get("link");
    const imagePath = value.get("image");

    if (name) {
      newEvent.name = name.toString();
    }

    if (description) {
      newEvent.description = description.toString();
    }

    if (link) {
      newEvent.link = link.toString();
    }

    if (imagePath) {
      const imageURL =
        "https://ipfs.io/ipfs/" +
        event.params.eventDataCID +
        imagePath.toString();
      newEvent.imageURL = imageURL;
    } else {
      // return fallback image if no imagePath
      const fallbackURL =
        "https://ipfs.io/ipfs/bafybeibssbrlptcefbqfh4vpw2wlmqfj2kgxt3nil4yujxbmdznau3t5wi/event.png";
      newEvent.imageURL = fallbackURL;
    }
  }
}

newEvent.save();
```

Notre fonction `handleNewEventCreated` devrait maintenant ressembler à ceci :

```javascript
export function handleNewEventCreated(event: NewEventCreated): void {
  let newEvent = Event.load(event.params.eventID.toHex());
  if (newEvent == null) {
    newEvent = new Event(event.params.eventID.toHex());
    newEvent.eventID = event.params.eventID;
    newEvent.eventOwner = event.params.creatorAddress;
    newEvent.eventTimestamp = event.params.eventTimestamp;
    newEvent.maxCapacity = event.params.maxCapacity;
    newEvent.deposit = event.params.deposit;
    newEvent.paidOut = false;
    newEvent.totalRSVPs = integer.ZERO;
    newEvent.totalConfirmedAttendees = integer.ZERO;

    let metadata = ipfs.cat(event.params.eventDataCID + "/data.json");

    if (metadata) {
      const value = json.fromBytes(metadata).toObject();
      if (value) {
        const name = value.get("name");
        const description = value.get("description");
        const link = value.get("link");
        const imagePath = value.get("image");

        if (name) {
          newEvent.name = name.toString();
        }

        if (description) {
          newEvent.description = description.toString();
        }

        if (link) {
          newEvent.link = link.toString();
        }

        if (imagePath) {
          const imageURL =
            "https://ipfs.io/ipfs/" +
            event.params.eventDataCID +
            imagePath.toString();
          newEvent.imageURL = imageURL;
        } else {
          const fallbackURL =
            "https://ipfs.io/ipfs/bafybeibssbrlptcefbqfh4vpw2wlmqfj2kgxt3nil4yujxbmdznau3t5wi/event.png";
          newEvent.imageURL = fallbackURL;
        }
      }
    }

    newEvent.save();
  }
}
```

Pour notre fonction `handleNewRSVP`, nous allons créer une nouvelle entité RSVP et une nouvelle entité compte (en supposant qu'il s'agisse d'un nouvel utilisateur). Pour garder nos fonctions simples, nous pouvons les diviser en deux fonctions.

```javascript
function getOrCreateAccount(address: Address): Account {
  let account = Account.load(address.toHex());
  if (account == null) {
    account = new Account(address.toHex());
    account.totalRSVPs = integer.ZERO;
    account.totalAttendedEvents = integer.ZERO;
    account.save();
  }
  return account;
}

export function handleNewRSVP(event: NewRSVP): void {
  let id = event.params.eventID.toHex() + event.params.attendeeAddress.toHex();
  let newRSVP = RSVP.load(id);
  let account = getOrCreateAccount(event.params.attendeeAddress);
  let thisEvent = Event.load(event.params.eventID.toHex());
  if (newRSVP == null && thisEvent != null) {
    newRSVP = new RSVP(id);
    newRSVP.attendee = account.id;
    newRSVP.event = thisEvent.id;
    newRSVP.save();
    thisEvent.totalRSVPs = integer.increment(thisEvent.totalRSVPs);
    thisEvent.save();
    account.totalRSVPs = integer.increment(account.totalRSVPs);
    account.save();
  }
}
```

Notre fonction `handleConfirmedAttendee` suivra le même schéma :

```javascript
export function handleConfirmedAttendee(event: ConfirmedAttendee): void {
  let id = event.params.eventID.toHex() + event.params.attendeeAddress.toHex();
  let newConfirmation = Confirmation.load(id);
  let account = getOrCreateAccount(event.params.attendeeAddress);
  let thisEvent = Event.load(event.params.eventID.toHex());
  if (newConfirmation == null && thisEvent != null) {
    newConfirmation = new Confirmation(id);
    newConfirmation.attendee = account.id;
    newConfirmation.event = thisEvent.id;
    newConfirmation.save();

    thisEvent.totalConfirmedAttendees = integer.increment(
      thisEvent.totalConfirmedAttendees
    );
    thisEvent.save();

    account.totalAttendedEvents = integer.increment(
      account.totalAttendedEvents
    );
    account.save();
  }
}
```

Pour notre fonction `handleDepositsPaidOut`, tout ce que nous devons faire est de changer le champ paidOut de l'événement correspondant de false à true :

```javascript
export function handleDepositsPaidOut(event: DepositsPaidOut): void {
  let thisEvent = Event.load(event.params.eventID.toHex());
  if (thisEvent) {
    thisEvent.paidOut = true;
    thisEvent.save();
  }
}
```

Vous pouvez voir une version finale du fichier de mappings [ici] (https://github.com/womenbuildweb3/web3RSVP-subgraph/blob/main/src/mapping.ts).

Enfin, nous pouvons lancer `graph build` dans le terminal pour construire notre subgraph et nous assurer qu'il n'y a pas d'erreurs.

## ✋ Need Help?

If you need help, check to see if your question has already been asked in **#section-5-help**. If you don't see it in there, post a question with any details that would make it easy for a team member to help you. We'll answer most frequently asked questions in live office hours, so keep an eye out in **#announcements** for those!

## Vous l'avez fait  🎉

Vous venez d'écrire les mappings pour votre subgraph - la partie la plus difficile est faite ! Envoyez un tweet et informez la communauté de vos progrès en cliquant sur le bouton Partager ci-dessous. Collez le lien vers votre tweet dans **#builders-hype** et faites du battage publicitaire pour tous les autres tweets que vous y verrez 🔥.

---

Écrivains: [Sarah Schwartz](https://twitter.com/schwartzswartz)
Rédacteurs: [Cami](https://twitter.com/camiinthisthang)
Traducteur: [CookingCryptos](https://twitter.com/CookingCryptos)
