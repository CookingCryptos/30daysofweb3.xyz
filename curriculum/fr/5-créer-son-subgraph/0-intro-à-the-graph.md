---
title: Introduction à The Graph
description: Learn about The Graph, a web3 protocol that enables developers to create APIs to query data from the blockchain.
optional: false
tweet: "Learn about @graphprotocol with #30DaysofWeb3 @womenbuildweb3 👾"
---

![Introduction à The Graph](https://user-images.githubusercontent.com/15064710/180662082-82426a3c-4a63-40d4-a30b-ce23bae7a38f.png)

The Graph est un protocole web3 qui permet aux développeurs de créer des API GraphQL pour interroger les données de n'importe quel contrat intelligent. Il est ainsi possible de créer rapidement et facilement des tableaux de bord et des sites Web qui affichent des données en direct sur la façon dont les utilisateurs interagissent avec votre contrat.

Tout le monde peut déployer sa propre API, également appelée subgraph. Notre subgraph nous permettra de connecter notre site Web frontend à notre contrat afin que nous puissions facilement récupérer les données dont nous avons besoin.

Sans The Graph, les développeurs devaient faire tourner leurs propres serveurs d'indexation pour pouvoir interroger les données de leurs smart contracts. Ce type de serveur peut s'avérer coûteux et, comme il s'agit d'un serveur centralisé, il peut être vulnérable aux interruptions de service et à la censure.

Vous pouvez utiliser le service hébergé de The Graph gratuitement avec n'importe quelle chaîne. Vous trouverez une liste complète [ici] (https://thegraph.com/hosted-service/). Vous pouvez également utiliser le réseau décentralisé de The Graph pour les subgraphes qui indexent les contrats du mainnet Ethereum, qui facture une petite somme pour chaque requête. Pour ce tutoriel, nous utiliserons le service hébergé gratuit.

> Remarque : pour vous faciliter la tâche, veuillez créer un nouveau dossier repo/projet pour cette section. Toutefois, si vous vous sentez suffisamment à l'aise, vous pouvez choisir d'utiliser [mono repos](https://blog.logrocket.com/managing-full-stack-monorepo-pnpm/).

## Installation du CLI

Pour commencer, vous devez installer The Graph CLI :

```
# NPM
$ npm install -g @graphprotocol/graph-cli

# Yarn
$ yarn global add @graphprotocol/graph-cli
```

Vous pouvez tester pour voir s'il a été installé correctement en exécutant :

```
graph -v
```

Pour cet exemple, nous utilisons la version 0.30.3.

## ✋ Need Help?

If you need help, check to see if your question has already been asked in **#section-5-help**. If you don't see it in there, post a question with any details that would make it easy for a team member to help you. We'll answer most frequently asked questions in live office hours, so keep an eye out in **#announcements** for those!

---

Écrivains: [Sarah Schwartz](https://twitter.com/schwartzswartz)
Rédacteurs: [Cami](https://twitter.com/camiinthisthang)
Traducteur: [CookingCryptos](https://twitter.com/CookingCryptos)
