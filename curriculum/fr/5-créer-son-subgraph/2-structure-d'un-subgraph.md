---
title: Structure d'un Subgraph
description: Check out the scaffold subgraph that you will use to create your subgraph.
optional: false
tweet: "Create and deploy a subgraph on @graphprotocol with #30DaysofWeb3 @womenbuildweb3 👾"
---

Vous devriez maintenant avoir un nouveau dossier avec cette structure :

```
subgraph-name
└───abis
│   │  {Name}.json
└───generated
│   └───{Name}
│       │   {Name}.ts
│   │   schema.ts
│   networks.json
│   package.json
│   schema.graphql
└───src
│  │   {Name}.ts
│   subgraph.yaml
│   tsconfig.json
│   yarn.lock
```

Assurez-vous d'exécuter `yarn` ou `npm install` pour installer les dépendances. Si vous utilisez git, ajoutez un fichier `.gitignore` pour ignorer le dossier `node_modules`.

Nous allons également ajouter `@protofire/subgraph-toolkit` comme dépendance. Lancez `yarn add @protofire/subgraph-toolkit` ou `npm install @protofire/subgraph-toolkit` pour ajouter ce paquet.

L'installation initiale devrait avoir déjà récupéré l'ABI du contrat pour vous. Si vous ne le voyez pas dans le dossier abis, vous pouvez trouver l'abi dans votre projet hardhat dans le dossier artifacts. Il sera appelé `[YourContractName].json`.

Les trois principaux fichiers du dossier du projet subgraph que nous allons modifier sont les fichiers `subgraph.yaml`, `schema.graphql`, et `src/{Name}.ts`.

## ✋ Need Help?

If you need help, check to see if your question has already been asked in **#section-5-help**. If you don't see it in there, post a question with any details that would make it easy for a team member to help you. We'll answer most frequently asked questions in live office hours, so keep an eye out in **#announcements** for those!

---

Écrivains: [Sarah Schwartz](https://twitter.com/schwartzswartz),
Rédacteurs: [Kristen](https://twitter.com/cuddleofdeath), [Briseida Montiel](https://twitter.com/brizism), [Cami](https://twitter.com/camiinthisthang)
Traducteur: [CookingCryptos](https://twitter.com/CookingCryptos)