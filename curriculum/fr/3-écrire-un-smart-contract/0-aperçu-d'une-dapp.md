@---
title: Aperçu d'une Dapp
description: With 30 Days of Web3, you will develop a full-stack decentralized event platform using must-know web3 tools, protocols, and frameworks including Solidity, The Graph, IPFS, Polygon, Ethers.js, and RainbowKit.
optional: false
tweet: "Ship a full-stack event platform dapp with #30DaysofWeb3 @womenbuildweb3 🎫"
---

![Dapp Overview](https://user-images.githubusercontent.com/15064710/180662009-26c933fc-6e3d-4f79-8fe2-c6346ced8e3c.png)

## Définir le but et les besoins

Pendant ces 30 jours, nous allons travailler à la création d'une dapp RSVP complète. Pensez-y comme un Eventbrite natif du web3, sauf que les participants doivent déposer de l'ETH pour RSVP et le récupérer lorsqu'ils se présentent à l'événement.

**Voici ce que notre Smart Contract devrait être en mesure de gérer :**

**Création d'un nouvel événement:**

- Un identifiant unique
- Une référence à la personne qui a créé l'événement (une adresse Wallet du créateur)
- L'heure de l'événement, afin que nous sachions quand les remboursements doivent être disponibles.
- La capacité maximale de participants pour cet événement
- Le montant de la caution pour cet événement
- Garder la trace de ceux qui ont répondu à l'invitation
- Garder la trace des utilisateurs qui se sont inscrits à l'événement
- Trouvez la différence entre ceux qui ont répondu à l'invitation et ceux qui se sont présentés, et remboursez tout le monde sauf ceux-là.

**RSVP à un événement:**

- Indiquez l'identifiant unique de l'événement auquel l'utilisateur souhaite répondre.
- Assurez-vous que la valeur de leur dépôt est suffisante pour le dépôt requis pour cet événement.
- Assurez-vous que l'événement n'a pas déjà commencé, d'après l'horodatage de l'événement - les personnes ne devraient pas être en mesure de répondre après le début de l'événement.
- Assurez-vous que l'événement ne dépasse pas sa capacité maximale.

**Enregistrement des participants:**

- Passez un ID d'événement unique pour l'événement pour lequel l'utilisateur veut confirmer les utilisateurs
- Assurez-vous que seul le créateur de l'événement peut confirmer les participants.
- Indiquez l'adresse de l'utilisateur qui s'inscrit et vérifiez si son adresse est enregistrée dans la liste RSVP.
- S'il figure dans la liste RSVP, ajoutez-le à la liste des participants qui se sont présentés.
- Si l'utilisateur figure dans la liste des participants qui se sont présentés, rendez-lui son dépôt.

**Retirez tous les dépôts restants des invités qui ne se sont pas présentés et envoyez-les au propriétaire de l'événement:**.

- Indiquez l'identifiant unique de l'événement pour lequel l'utilisateur souhaite retirer des fonds.
- Assurez-vous qu'au moins 7 jours se sont écoulés depuis l'événement.
- Trouvez la différence entre le nombre d'invités qui se sont inscrits et le nombre d'invités qui se sont présentés.
- Multipliez le montant du dépôt par l'écart entre le nombre d'invités qui se sont inscrits et le nombre d'invités qui se sont présentés et renvoyez le montant au propriétaire.

### Events et subgraphs

Lors de l'écriture de notre Smart Contract, nous allons créer des events personnalisés qui nous aideront à construire notre subgraph. **Les events** sont des _déclencheurs que votre frontend et votre subgraph peuvent écouter_, ce qui les rend parfaits pour exécuter conditionnellement du code dans le frontend et indexer conditionnellement de la data.

Le subgraph est ce qui nous permettra de créer la possibilité d'ajouter une fonctionnalité comme un tableau de bord, où les utilisateurs pourrons voir les événements qu'ils ont créés et/ou auxquels ils ont participé. Nous reviendrons plus tard sur les subgraphs, mais pour l'instant, il est important de noter que nous devons exposer les données relatives aux événements qu'un utilisateur crée et auxquels il participe via des events Solidity.

**Les events que nous allons créer:**

- **NewRSVP:** à émettre à l'intérieur de la fonction qui est appelée lorsqu'un utilisateur s'inscrit à un événement.
- **ConfirmedAttendee:** à émettre à l'intérieur de la fonction qui est appelée lorsqu'un utilisateur s'inscrit à un événement.
- **DepositsPaid:** à émettre dans la fonction qui est appelée lorsqu'un utilisateur s'inscrit à un événement et récupère son dépôt.

### Préparation du subgraph

Nous allons plonger dans les détails de la construction de notre subgraph après la section sur le smart contract, mais vous devez comprendre le rôle d'un subgraph dans votre application lorsque vous écrivez le smart contract pour vous assurer que vous optimisez votre contrat pour les informations on-chain que vous voulez afficher sur votre front-end.

Le **subgraph** vous permet de _poser des questions sophistiquées_ sur votre data. Le subgraph n'a accès qu'aux données que nous exposons via les _events Solidity_. Pour cette raison, nous allons créer des events personnalisés qui exposent de la data. Nous poserons des questions concernant notre data pour obtenir des informations sur les événements pour lesquels nos utilisateurs sont RSVP, les événements qui sont dans le futur vs les événements qui sont déjà passés.

## ✋ Need Help?

Now that we're in section 3 of the curriculum, first check to see if your question has already been answered in **#section-3-help**. If you don't see it there, drop your question along with any details that could help us help you :)

---

Écrivain: [Cami](https://twitter.com/camiinthisthang)
Traducteur: [CookingCryptos](https://twitter.com/CookingCryptos)
