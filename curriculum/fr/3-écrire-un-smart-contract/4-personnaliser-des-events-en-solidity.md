---
title: Events personnalisés en Solidity
description: Define Solidity events to listen for certain actions on the blockchain.
optional: false
tweet: "I just wrote a production grade smart contract 🎉 #30DaysofWeb3 @womenbuildweb3 ✍️"
---

## Définir les événements

Rappelons que les events en Solidity sont un moyen pour notre subgraph d'_écouter_ des actions spécifiques pour nous permettre d'effectuer des requêtes sur les données de notre smart contract. Nous avons des fonctions écrites pour créer un nouvel événement sur notre plateforme, répondre à un événement, confirmer les participants individuels, confirmer le groupe de participants, et renvoyer les fonds non réclamés au propriétaire de l'événement. Pour que notre subgraph puisse accéder aux informations de ces fonctions, nous devons les exposer via des events. Nous allons écrire les events suivants et les _émettre_ à l'intérieur de leur fonction correspondante :

- **NewEventCreated** : expose des données sur le nouvel événement comme le propriétaire, la capacité maximale, le propriétaire de l'événement, le montant du dépôt, etc.
- **NewRSVP** : expose des données sur l'utilisateur qui a RSVP et l'événement pour lequel il a RSVP.
- **ConfirmedAttendee** : expose les données sur l'utilisateur qui a été confirmé et l'événement pour lequel il a été confirmé.
- **DepositsPaid** : expose les données sur les dépôts non réclamés envoyés à l'organisateur de l'événement.

Tous nos events sont désignés par le mot-clé _event_, suivi du nom personnalisé de notre event.

Définissez vos events au tout début de votre fichier, entre les accolades (juste après `contract Web3RSVP {`):

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

## Émettre des Events

Maintenant que nous les avons définis, nous devons utiliser _emit_ quelque part. Définir les events, c'est les ajouter comme un outil à votre ceinture d'outils, mais les émettre, c'est sortir cet outil et l'utiliser. Chaque event doit être être utilisé avec un emit là où il a du sens, après qu'une action spécifique ait été effectuée.

Pour notre premier event, `newEventCreated`, nous devons utiliser emit à la toute fin de notre fonction `createNewEvent`.

Pour émettre un événement, nous utilisons le mot-clé _emit_ et nous passons ensuite les arguments, c'est-à-dire les valeurs réelles que nous voulons, en fonction des paramètres que nous avons définis.

Emettez NewEventCreated à la fin de votre fonction `createNewEvent` comme ceci :

```solidity
emit NewEventCreated(
    eventId,
    msg.sender,
    eventTimestamp,
    maxCapacity,
    deposit,
    eventDataCID
);
```

`NewRSVP` doit être émis à la toute fin de notre fonction `createNewRSVP` comme ceci : 

```solidity
emit NewRSVP(eventId, msg.sender);

```

`ConfirmedAttendees` doit être émis à la toute fin de notre fonction `confirmAttendee` comme ceci :

```solidity
emit ConfirmedAttendee(eventId, attendee);

```

`DepositPaidOut` doit être émis à la toute fin de notre fonctio `withdrawUnclaimedDeposits` comme ceci :

```solidity
emit DepositsPaidOut(eventId);

```

## Vous l'avez fait !

Vous avez écrit vos fonctions, défini vos structures, et défini des events personnalisés ! Voici à quoi devrait ressembler votre code final :

```solidity
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.4;

contract Web3RSVP {

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
        // génére un eventID unique en combinant des elements et en générant un hash
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


        //cela crée un nouveau struct CreateEvent et l'ajoute au mapping idToEvent
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

        emit NewEventCreated(
            eventId,
            msg.sender,
            eventTimestamp,
            maxCapacity,
            deposit,
            eventDataCID
        );
    }

    function createNewRSVP(bytes32 eventId) external payable {
        // recherche d'un événement
        CreateEvent storage myEvent = idToEvent[eventId];

        // transfére le dépôt sur notre contrat / exiger qu'ils envoient suffisamment d'ETH
        require(msg.value == myEvent.deposit, "NOT ENOUGH");

        // exige que l'événement n'ait pas déjà eu lieu (<eventTimestamp)
        require(block.timestamp <= myEvent.eventTimestamp, "ALREADY HAPPENED");

        // s'assurer que l'événement est sous la capacité maximale
        require(
            myEvent.confirmedRSVPs.length < myEvent.maxCapacity,
            "This event has reached capacity"
        );

        // exige que msg.sender ne soit pas déjà dans myEvent.confirmedRSVPs
        for (uint8 i = 0; i < myEvent.confirmedRSVPs.length; i++) {
            require(myEvent.confirmedRSVPs[i] != msg.sender, "ALREADY CONFIRMED");
        }

        myEvent.confirmedRSVPs.push(payable(msg.sender));

        emit NewRSVP(eventId, msg.sender);
    }

    function confirmAllAttendees(bytes32 eventId) external {
        // recherche d'un événement
        CreateEvent memory myEvent = idToEvent[eventId];

        // exige que msg.sender soit le propriétaire de l'événement
        require(msg.sender == myEvent.eventOwner, "NOT AUTHORIZED");

        // confirme chaque participant
        for (uint8 i = 0; i < myEvent.confirmedRSVPs.length; i++) {
            confirmAttendee(eventId, myEvent.confirmedRSVPs[i]);
        }
    }

    function confirmAttendee(bytes32 eventId, address attendee) public {
        // recherche de l'événement
        CreateEvent storage myEvent = idToEvent[eventId];

        // exige que msg.sender soit le propriétaire de l'événement
        require(msg.sender == myEvent.eventOwner, "NOT AUTHORIZED");

        // exigez que l'invité figure dans myEvent.confirmedRSVPs
        address rsvpConfirm;

        for (uint8 i = 0; i < myEvent.confirmedRSVPs.length; i++) {
            if(myEvent.confirmedRSVPs[i] == attendee){
                rsvpConfirm = myEvent.confirmedRSVPs[i];
            }
        }

        require(rsvpConfirm == attendee, "NO RSVP TO CONFIRM");


        // exige que l'attendee ne figure PAS dans la liste des claimedRSVP
        for (uint8 i = 0; i < myEvent.claimedRSVPs.length; i++) {
            require(myEvent.claimedRSVPs[i] != attendee, "ALREADY CLAIMED");
        }

        // exige que les dépôts ne soient pas déjà réclamés
        require(myEvent.paidOut == false, "ALREADY PAID OUT");

        // les ajouter à la liste des claimedRSVP
        myEvent.claimedRSVPs.push(attendee);

        // renvoyer eth au staker https://solidity-by-example.org/sending-ether
        (bool sent,) = attendee.call{value: myEvent.deposit}("");

        // si cela échoue
        if (!sent) {
            myEvent.claimedRSVPs.pop();
        }

        require(sent, "Failed to send Ether");

        emit ConfirmedAttendee(eventId, attendee);
    }

    function withdrawUnclaimedDeposits(bytes32 eventId) external {
        // recherche de l'événement
        CreateEvent memory myEvent = idToEvent[eventId];

        // vérifie si vous avez déjà payé
        require(!myEvent.paidOut, "ALREADY PAID");

        // vérifie si cela fait 7 jours depuis myEvent.eventTimestamp
        require(
            block.timestamp >= (myEvent.eventTimestamp + 7 days),
            "TOO EARLY"
        );

        // seul le propriétaire de l'événement peut se retirer
        require(msg.sender == myEvent.eventOwner, "MUST BE EVENT OWNER");

        // calculer combien de personnes n'ont pas réclamé en comparant
        uint256 unclaimed = myEvent.confirmedRSVPs.length - myEvent.claimedRSVPs.length;

        uint256 payout = unclaimed * myEvent.deposit;

        // marquez comme payé avant l'envoi pour éviter une reentrancy attack
        myEvent.paidOut = true;

        // envoie le paiement au propriétaire
        (bool sent, ) = msg.sender.call{value: payout}("");

        // si cela échoue
        if (!sent) {
            myEvent.paidOut = false;
        }

        require(sent, "Failed to send Ether");

        emit DepositsPaidOut(eventId);
    }
}
```

## ✋ Need Help?

If you need help, check to see if your question has already been asked in **#section-3-help**. If you don't see it in there, post a question with any details that would make it easy for a team member to help you. We'll answer most frequently asked questions in live office hours, so keep an eye out in **#announcements** for those!

## 🏝 Temps de pause

Vous venez d'écrire des événements personnalisés et de définir toutes les fonctions nécessaires au fonctionnement de Web3RSVP ! Appuyez sur le bouton Partager ci-dessous pour tweeter et participer à la chance de gagner l'un de nos prix 👀. Collez votre tweet dans **#builders-hype**, et nous choisirons un gagnant dans les prochaines 72 heures.

**Fin du 4ème jour**

---

Écrivains: [Cami](https://twitter.com/camiinthisthang)
Traducteur: [CookingCryptos](https://twitter.com/CookingCryptos)