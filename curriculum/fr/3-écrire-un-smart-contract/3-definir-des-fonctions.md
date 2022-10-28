---
title: Définir les fonctions
description: Define functions in Solidity to handle events in your full-stack decentralized event platform.
optional: false
tweet: "Write a smart contract in Solidity for a full-stack dapp with #30DaysofWeb3 @womenbuildweb3 ✍️"
---

## Créer un nouvel événement

Ensuite, nous allons écrire la fonction qui sera appelée lorsqu'un utilisateur crée un nouvel événement dans notre frontend. Il s'agit d'une méthode setter - une fonction qui s'exécute et définit la valeur en fonction des informations fournies par l'utilisateur.

**A reminder of what this function should be able to handle:**

- Un identifiant unique
- Une référence à la personne qui a créé l'événement (une adresse Wallet du créateur)
- L'heure de l'événement, afin que nous sachions quand les remboursements doivent être disponibles.
- La capacité maximale de participants pour cet événement
- Le montant de la caution pour cet événement
- Garder la trace de ceux qui ont répondu à l'invitation
- Garder la trace des utilisateurs qui se sont inscrits à l'événement

Après avoir ajouté cette fonction à notre mapping, voici à quoi votre Smart Contract devrait ressembler jusqu'à présent :

```solidity
struct CreateEvent {
    bytes32 eventId;
    string eventDataCID;
    address eventOwner;
    uint256 eventTimestamp;
    uint256 deposit;
    uint256 maxCapacity;
    address[] confirmedRSVPs;
    address[] claimedRSVPs;
    bool paidOut;
}

mapping(bytes32 => CreateEvent) public idToEvent;

function createNewEvent(
    uint256 eventTimestamp,
    uint256 deposit,
    uint256 maxCapacity,
    string calldata eventDataCID
) external {
    // générer un identifiant d'événement basé sur d'autres éléments transmis pour générer un hash
    bytes32 eventId = keccak256(
        abi.encodePacked(
            msg.sender,
            address(this),
            eventTimestamp,
            deposit,
            maxCapacity
        )
    );

    address[] memory confirmedRSVPs;
    address[] memory claimedRSVPs;


    // Ceci crée une nouvelle structure CreateEvent et l'ajoute à l'idToEvent.  mapping
    idToEvent[eventId] = CreateEvent(
        eventId,
        eventDataCID,
        msg.sender,
        eventTimestamp,
        deposit,
        maxCapacity,
        confirmedRSVPs,
        claimedRSVPs,
        false
    );
}

```

**Explication ligne par ligne de ce que nous venons de faire:**

Nous définissons la fonction `createNewEvent` et définissons les paramètres que la fonction doit accepter. Ce sont les paramètres spécifiques à un événement que nous obtiendrons de la personne qui crée réellement l'événement sur le frontend. Ces choses sont : l'eventTimestamp (quand l'événement commencera), le dépôt requis pour RSVP à cet événement, la capacité maximale de cet événement, et une référence au hash IPFS contenant des informations comme le nom et la description de l'événement.

```solidity
function createNewEvent(
    uint256 eventTimestamp,
    uint256 deposit,
    uint256 maxCapacity,
    string calldata eventDataCID
)
```

Nous avons mis la visibilité de la fonction en externe car elle est très performante et économise du gas.

`external {`

Dans le corps de la fonction, nous créons un ID unique pour l'événement en **hachant** quelques valeurs.

Nous laissons l'utilisateur entrer son propre identifiant unique, mais cela peut conduire à des **collisions**. Les collisions sont un problème auquel il faut faire attention lors de la création de tables de hachage ou de mappings. Les collisions se produisent lorsque deux éléments ou plus sont référencés par le même hachage (ID unique).

Par exemple, si deux utilisateurs créent un événement au même moment, ils peuvent tous deux obtenir le même identifiant, ou ils peuvent choisir sans le savoir le même identifiant que quelqu'un d'autre. Cela poserait des problèmes plus tard lorsque nous essaierions de rechercher des éléments par leur ID d'événement unique.

Afin de combattre ce problème, nous générons un identifiant unique en créant un hachage en passant dans tous les arguments passés dans l'appel de fonction. La combinaison de tous les arguments et de la fonction de hachage réduit considérablement la probabilité d'une collision.

```solidity
// générer un identifiant d'événement basé sur d'autres éléments transmis pour générer un hash
bytes32 eventId = keccak256(
    abi.encodePacked(
        msg.sender,
        address(this),
        eventTimestamp,
        deposit,
        maxCapacity
    )
);
```

Pour éviter une collision ici, vous pouvez ajouter une instruction require pour vous assurer que l'eventId n'est pas déjà utilisé.

```javascript
// assurez-vous que cet identifiant n'est pas déjà réclamé
require(idToEvent[eventId].eventTimestamp == 0, "ALREADY REGISTERED");
```

Nous initialisons les deux tableaux que nous utiliserons pour suivre les RSVP et les participants. Nous savons que nous devons définir ces deux tableaux parce que dans notre struct, CreateEvent, nous définissons qu'il y aura deux tableaux qui seront utilisés. Un pour suivre les adresses des utilisateurs qui ont répondu à l'invitation. L'autre pour suivre l'adresse des utilisateurs qui arrivent effectivement et sont enregistrés dans l'événement, c'est-à-dire sont confirmés.

```solidity
address[] memory confirmedRSVPs;
address[] memory claimedRSVPs;
```

Maintenant que nous avons un identifiant unique, nous pouvons créer une nouvelle entrée dans notre mapping. Vous pouvez considérer cela comme l'ajout d'un nouvel événement à notre répertoire d'événements gérés par ce Smart Contract.

La `key` est l'eventID et la `value` est un struct, ou un objet, avec les propriétés suivantes que nous avons saisies soit à partir des arguments de fonction passés par l'utilisateur dans le front-end (eventName, eventTimestamp, deposit, maxCapacity), certaines que nous avons générées nous-mêmes, ou recueillies du côté du smart contract (eventID, eventOwner, confirmedRSVPS, claimedRSVPs). Enfin, nous mettons le booléen (bool) paidOut à false parce qu'au moment de la création de l'événement, aucun paiement n'a encore été effectué aux RSVP (il n'y en a pas encore) ou au propriétaire de l'événement.

```solidity
idToEvent[eventId] = CreateEvent(
    eventId,
    eventName,
    msg.sender,
    eventTimestamp,
    deposit,
    maxCapacity,
    confirmedRSVPs,
    claimedRSVPs,
    false
);
```

## ✋ Need Help?

If you need help, check to see if your question has already been asked in **#section-3-help**. If you don't see it in there, post a question with any details that would make it easy for a team member to help you. We'll answer most frequently asked questions in live office hours, so keep an eye out in **#announcements** for those!

## RSVP à l'événement

Ensuite, nous allons écrire la fonction qui est appelée lorsqu'un utilisateur trouve un événement et y répond sur le front-end.

**Rappel des exigences pour une fonction permettant aux utilisateurs de répondre à un événement**

- Transmettre l'identifiant unique de l'événement auquel l'utilisateur souhaite répondre.
- Assurez-vous que la valeur de leur dépôt est suffisante pour l'exigence de dépôt de cet événement.
- S'assurer que l'événement n'a pas déjà commencé en se basant sur l'horodatage de l'événement - les personnes ne devraient pas être en mesure de RSVP après le début de l'événement.
- S'assurer que l'événement ne dépasse pas sa capacité maximale.
- Ajoutez cette fonction à votre contrat, juste en dessous de la fonction createNewEvent.

Voici à quoi ressemblera votre fonction createNewRSVP :

```solidity
function createNewRSVP(bytes32 eventId) external payable {
    // rechercher un événement dans notre mapping
    CreateEvent storage myEvent = idToEvent[eventId];

    // transférer le dépôt sur notre contrat / exiger qu'ils envoient suffisamment d'ETH pour couvrir le dépôt requis pour cet événement spécifique
    require(msg.value == myEvent.deposit, "NOT ENOUGH");

    // exiger que l'événement n'ait pas déjà eu lieu (<eventTimestamp)
    require(block.timestamp <= myEvent.eventTimestamp, "ALREADY HAPPENED");

    // s'assurer que l'événement ne dépasse pas la capacité maximale
    require(
        myEvent.confirmedRSVPs.length < myEvent.maxCapacity,
        "This event has reached capacity"
    );

    // il faut que msg.sender ne soit pas déjà dans myEvent.confirmedRSVPs AKA qu'il n'ait pas déjà RSVP.
    for (uint8 i = 0; i < myEvent.confirmedRSVPs.length; i++) {
        require(myEvent.confirmedRSVPs[i] != msg.sender, "ALREADY CONFIRMED");
    }

    myEvent.confirmedRSVPs.push(payable(msg.sender));

}
```

## Enregistrement des participants  

Une partie de notre application demande aux utilisateurs de payer une caution qu'ils récupèrent à leur arrivée à l'événement. Nous allons écrire la fonction qui enregistre les participants et leur renvoie leur caution.

```solidity
function confirmAttendee(bytes32 eventId, address attendee) public {
    // rechercher un événement dans notre struct en utilisant l'eventId
    CreateEvent storage myEvent = idToEvent[eventId];

    // exiger que msg.sender soit le propriétaire de l'événement - seul l'hôte devrait être en mesure d'enregistrer les personnes
    require(msg.sender == myEvent.eventOwner, "NOT AUTHORIZED");

    // exiger que le participant qui essaie de s'enregistrer ait réellement répondu au RSVP
    address rsvpConfirm;

    for (uint8 i = 0; i < myEvent.confirmedRSVPs.length; i++) {
        if(myEvent.confirmedRSVPs[i] == attendee){
            rsvpConfirm = myEvent.confirmedRSVPs[i];
        }
    }

    require(rsvpConfirm == attendee, "NO RSVP TO CONFIRM");


    // s'assurer que le participant ne figure PAS déjà dans la liste des claimedRSVPs, c'est-à-dire vérifier qu'il ne s'est pas déjà enregistré
    for (uint8 i = 0; i < myEvent.claimedRSVPs.length; i++) {
        require(myEvent.claimedRSVPs[i] != attendee, "ALREADY CLAIMED");
    }

    // exiger que les dépôts ne soient pas déjà réclamés par le propriétaire de l'événement
    require(myEvent.paidOut == false, "ALREADY PAID OUT");

    // ajouter le participant à la liste claimRSVPs
    myEvent.claimedRSVPs.push(attendee);

    // renvoi d'eth au staker `https://solidity-by-example.org/sending-ether`
    (bool sent,) = attendee.call{value: myEvent.deposit}("");

    // si cela échoue, retirer l'utilisateur du tableau des RSVP réclamés
    if (!sent) {
        myEvent.claimedRSVPs.pop();
    }

    require(sent, "Failed to send Ether");
}
```

## Confirmer l'ensemble du groupe

En tant qu'organisateur d'événements, vous souhaitez peut-être pouvoir confirmer tous vos participants en une seule fois, au lieu de les traiter un par un.

Écrivons une fonction pour confirmer toutes les personnes qui ont répondu à un événement spécifique :

```solidity
function confirmAllAttendees(bytes32 eventId) external {
    // recherche l'événement dans notre structure avec l'eventId
    CreateEvent memory myEvent = idToEvent[eventId];

    // assurez-vous que vous exigez que msg.sender soit le propriétaire de l'événement
    require(msg.sender == myEvent.eventOwner, "NOT AUTHORIZED");

    // confirmer chaque participant dans le tableau rsvp
    for (uint8 i = 0; i < myEvent.confirmedRSVPs.length; i++) {
        confirmAttendee(eventId, myEvent.confirmedRSVPs[i]);
    }
}
```

## Envoyer les dépôts non réclamés à l'organisateur de l'événement

Enfin, nous devons écrire une fonction qui retirera les dépôts des personnes qui ne se sont pas présentées à l'événement et les enverra à l'organisateur de l'événement :
```solidity
function withdrawUnclaimedDeposits(bytes32 eventId) external {
    // recherche de l'événement
    CreateEvent memory myEvent = idToEvent[eventId];

    // vérifie que le booléen paidOut est toujours égal à false, c'est-à-dire que l'argent n'a pas déjà été versé
    require(!myEvent.paidOut, "ALREADY PAID");

    // vérifiez si 7 jours se sont écoulés depuis myEvent.eventTimestamp
    require(
        block.timestamp >= (myEvent.eventTimestamp + 7 days),
        "TOO EARLY"
    );

    // seul le propriétaire de l'événement peut le retirer
    require(msg.sender == myEvent.eventOwner, "MUST BE EVENT OWNER");

    // calculez combien de personnes ne se sont pas présentées en comparant les chiffres suivants
    uint256 unclaimed = myEvent.confirmedRSVPs.length - myEvent.claimedRSVPs.length;

    uint256 payout = unclaimed * myEvent.deposit;

    // marquer comme payé avant l'envoi pour éviter la "reentrancy attack"
    myEvent.paidOut = true;

    //  envoie le paiement au propriétaire
    (bool sent, ) = msg.sender.call{value: payout}("");

    // si cela échoue
    if (!sent) {
        myEvent.paidOut = false;
    }

    require(sent, "Failed to send Ether");

}
```

---

Écrivains: [Cami](https://twitter.com/camiinthisthang), Editors: [Sarah Schwartz](https://twitter.com/schwartzswartz), [Frego](https://twitter.com/cizeon)
Traducteur: [CookingCryptos](https://twitter.com/CookingCryptos)
