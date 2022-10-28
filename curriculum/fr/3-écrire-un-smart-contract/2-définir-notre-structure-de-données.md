---
title: Définir notre structure de data
description: Define structs in Solidity to represent events in your full-stack decentralized event platform.
optional: false
tweet: "Write a smart contract in Solidity for a full-stack dapp with #30DaysofWeb3 @womenbuildweb3 ✍️"
---

## Définissez un "Événements".

Commençons par définir notre smart contract et les informations que nous voulons stocker on-chain : la création d'un nouvel événement par un organisateur d'événements et les détails associés à cet événement. Nous allons enregistrer ces informations dans une structure (struct). Pour rappel, un struct est similaire à un object JS dans la mesure où il stocke des informations relatives à une entité. Dans notre cas, nous stockons les informations relatives à la création d'un nouvel événement sur notre contrat RSVP.

```solidity
contract Web3RSVP {
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
}
```

Ces propriétés sont les propriétés que chaque événement individuel aura sur notre plateforme. Tous ces détails seront également stockés sur la chaîne.

En général, il est sage de faire attention aux données que vous stockez on-chain, car le stockage des données sur Ethereum est coûteux. Pour cette raison, vous remarquerez que nous ne stockons pas les détails comme le nom de l'événement et la description de l'événement directement dans le struct, mais nous stockons une référence à un hachage IPFS où ces détails seront stockés off-chain. Nous y reviendrons plus tard, mais pour l'instant, sachez que c'est à cela que sert `eventDataCID` !

### Gérer des événements multiples

Parce que nous voulons que notre contrat soit capable de gérer la création de plusieurs événements, **nous avons besoin d'un mécanisme pour stocker et rechercher facilement les événements par un identifiant**, comme un eventID unique. C'est ce que nous utiliserons pour indiquer à notre programme à quel événement un utilisateur a répondu, puisque nous pouvons supposer qu'il y en aura plusieurs.

Pour ce faire, nous pouvons créer un `mapping` Solidity qui fait correspondre, ou définit une relation avec, un eventID unique à la structure que nous venons de définir et qui est associée à cet événement.

Nous utiliserons ce mapping pour nous assurer que nous faisons référence au bon événement lorsque nous appelons des fonctions sur cet événement, comme la confirmation des participants, etc.

À l'intérieur de notre contrat et sous notre struct, nous allons définir ce mapping.

```solidity
contract Web3RSVP {
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

}
```

Vous avez mis en place la structure de données que vous utiliserez pour contenir toutes les informations relatives à un événement créé sur votre plateforme, ainsi qu'un mapping pour vous aider à suivre les événements via un eventID unique. C'est un bon endroit pour faire une pause !

**Fin du 3ème jour**

---

Écrivain: [Cami](https://twitter.com/camiinthisthang)
Traducteur: [CookingCryptos](https://twitter.com/CookingCryptos)
