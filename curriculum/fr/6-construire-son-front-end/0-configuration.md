---
title: Construire le Frontend
description: Setup the frontend for your full-stack decentralized event platform.
optional: false
tweet: "Build a frontend for a full-stack dapp with #30DaysofWeb3 @womenbuildweb3 🖼"
---

![Building Your Frontend](https://user-images.githubusercontent.com/15064710/180662656-aa2264ea-961c-4bac-9c9c-8955a4296d36.png)

## Introduction

Dans cette leçon, nous allons construire un frontend pour notre application en utilisant React, Next.js, ethers.js, Rainbowkit, Web3.Storage, et The Graph. Notre application fonctionnera avec Coinbase Wallet ou d'autres portefeuilles contrôlés par l'utilisateur comme MetaMask, Rainbow et WalletConnect. Les utilisateurs seront en mesure de connecter leur Wallet et d'interagir avec notre Smart Contract afin de créer de nouveaux événements, de répondre à des événements et de confirmer les participants.

Dans les prochaines sections, nous allons nous efforcer de permettre aux utilisateurs de créer de nouveaux événements. Tout d'abord, nous allons configurer RainbowKit pour permettre une expérience multi-portefeuille intuitive dans notre application. Ensuite, nous intégrerons Web3.Storage pour stocker certaines données d'événements off-chain, ou hors blockchain. Puis nous importerons et utiliserons ethers.js pour interagir avec notre Smart Contract déployé. Enfin, nous allons préparer notre frontend pour appeler la fonction `createNewEvent` de notre smart contract et gérer les transactions réussies ou échouées.

> Note : Pour vous faciliter la tâche, veuillez créer un nouveau dossier repo/projet pour cette section. Cependant, si vous vous sentez suffisamment à l'aise, vous pouvez choisir d'utiliser [mono repos](https://blog.logrocket.com/managing-full-stack-monorepo-pnpm/).

## Installation

Pour commencer, vous pouvez forker et cloner notre repo de démarrage, qui contient quelques éléments de design pour rendre notre application un peu plus jolie.

- Allez sur le dépôt en cliquant sur [here](https://github.com/womenbuildweb3/web3RSVP-frontend-starter), puis cliquez sur le bouton **fork**. Vous avez maintenant créé votre propre fork (votre propre version de ce projet).
- Maintenant, vous devez obtenir le code localement sur votre machine. Cliquez sur le bouton **clone** et copiez le lien qui apparaît dans la liste déroulante.
- Dans votre terminal, dans le bon dossier (Desktop), exécutez ceci : `git clone https://github.com/...` (collez le lien que vous avez copié). Si vous avez réussi, vous devriez voir quelque chose qui dit : `Cloning into 'web3RSVP-frontend-starter'...`.
- Ensuite, assurez-vous de changer de répertoire dans le nouveau dossier en exécutant la commande `cd web3RSVP-frontend-starter` dans votre terminal.

Installez toutes les dépendances nécessaires à ce projet en exécutant `npm install` ou `yarn`. Vous pouvez démarrer le serveur de développement en exécutant `npm run dev` ou `yarn run dev`.

Maintenant vous avez ce code sur votre ordinateur ! Ouvrez ce dossier dans VS code soit manuellement, soit en entrant la commande `code .` dans le terminal (Note : Sur certains systèmes Linux, la commande `code .` peut nécessiter une installation supplémentaire si votre VSCode n'a pas été précédemment installé via le terminal).

Si vous souhaitez modifier l'un des dessins ou l'une des images utilisés en cours de route, allez-y ! Ce projet est 100% vôtre pour le personnaliser comme vous le souhaitez.

## ✋ Need Help?

If you need help, check to see if your question has already been asked in **#section-6-help**. If you don't see it in there, post a question with any details that would make it easy for a team member to help you. We'll answer most frequently asked questions in live office hours, so keep an eye out in **#announcements** for those!

---

Écrivains: [Avni Agrawal](https://twitter.com/AvniAgrawal1802),
Rédacteurs: [Sarah Schwartz](https://twitter.com/schwartzswartz),
Traducteur: [CookingCryptos](https://twitter.com/CookingCryptos)
