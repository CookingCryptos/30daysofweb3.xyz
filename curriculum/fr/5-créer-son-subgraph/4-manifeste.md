---
title: Manifeste du subgraph
description: Define the datasources that your subgraph will index.
optional: false
tweet: "Create and deploy a subgraph on @graphprotocol with #30DaysofWeb3 @womenbuildweb3 👾"
---

Le manifeste du subgraph (subgraph.yaml) est l'endroit où vous pouvez définir les paramètres du subgraph. La plupart de ces champs seront déjà remplis pour vous, mais il y a quelques changements que nous devons faire.

Juste au-dessus de `dataSources`, nous devons ajouter une section `features` où nous pouvons ajouter `ipfsOnEthereumContracts`.

```yaml
features:
  - ipfsOnEthereumContracts
```

Sous `dataSources`, vous pouvez voir l'adresse et l'abi de notre contrat. Nous pouvons ajouter une autre propriété ici appelée startBlock, que nous allons définir au numéro de bloc lorsque ce contrat a été déployé pour la première fois. L'ajout d'un bloc de départ réduit le temps nécessaire à l'indexation des données de votre subgraph.

Vous pouvez trouver le bloc de départ sur polygonscan. Copiez le numéro de bloc pour la première transaction lorsque le contrat est créé.

Le haut de notre `dataSources` devrait maintenant ressembler à ceci :

```yaml
dataSources:
  - kind: ethereum
    name: Web3RSVP
    network: mumbai
    source:
      address: "0xYOUR_ADDRESS_HERE"
      abi: Web3RSVP
      startBlock: 26(...YOUR_START_BLOCK)
```

Nous voulons aussi mettre à jour les noms de nos `Entity`. Vous pouvez supprimer les entités générées ici et les remplacer par celles ci-dessous. Nous allons créer quatre entités : `Event`, `Account`, `RSVP`, et `Confirmation`.

```yaml
entities:
  - Event
  - Account
  - RSVP
  - Confirmation
```

La section `eventHandlers` est l'endroit où nous pouvons indiquer au subgraph comment connecter chacun de nos mappings à différents déclencheurs d'events. Elle devrait déjà être remplie pour vous. Chaque fois qu'un event défini ici est emit par notre contrat, la fonction de mapping correspondante, définie comme handler, sera exécutée.

## ✋ Need Help?

If you need help, check to see if your question has already been asked in **#section-5-help**. If you don't see it in there, post a question with any details that would make it easy for a team member to help you. We'll answer most frequently asked questions in live office hours, so keep an eye out in **#announcements** for those!

---

Écrivains: [Sarah Schwartz](https://twitter.com/schwartzswartz)
Traducteur: [CookingCryptos](https://twitter.com/CookingCryptos)

