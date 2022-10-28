---
title: Déploiement vers le service hébergé
description: Deploy your subgraph to The Graph's Hosted Service.
optional: false
tweet: "Create and deploy a subgraph on @graphprotocol with #30DaysofWeb3 @womenbuildweb3 👾"
---

Ouvrez le subgraph que vous avez créé sur le service hébergé de The Graph, et sélectionnez le bouton "Show commands" sous Deploy.

Copiez la première commande qui commence par graph auth, et remplacez le `<ACCESS_TOKEN>` par votre token d'accès de la page de votre subgraph.

```
graph auth --product hosted-service <ACCESS_TOKEN>
```

L'exécution de cette commande vous authentifiera en tant que propriétaire du subgraph afin que vous puissiez pousser vos modifications de code. Collez et exécutez cette commande dans votre terminal à partir du dossier racine de votre projet. Vous devriez voir un message de confirmation dans votre terminal qui dit "Deploy key set".

Maintenant nous pouvons déployer notre subgraph en copiant la deuxième commande qui commence par graph deploy. Ici, il suffit de remplacer 
`<GITHUB_USER>/<SUBGRAPH NAME>` (vous pouvez le trouver dans l'url de votre subgraph).

```
graph deploy --product hosted-service <GITHUB_USER>/<SUBGRAPH NAME>
```

Exécutez cette commande pour déployer votre subgraph sur le service hébergé de The Graph.

Une fois que vous avez déployé, vous pouvez commencer à faire des requêtes tout de suite, cependant les données ne seront pas vérifiables jusqu'à ce que le subgraph ait terminé l'indexation.

### Mise à jour de votre subgraph

Si vous apportez des modifications à votre smart contract et le redéployez à une nouvelle adresse, vous pouvez facilement mettre à jour votre subgraph pour indexer le nouveau contrat.

Vous pouvez mettre à jour l'adresse du contrat dans le manifeste (subgraph.yaml), et copier et coller le nouvel abi dans le dossier abis. Si vous avez apporté des modifications au schéma ou émis des events à partir de votre contrat, assurez-vous d'exécuter graph codegen pour générer de nouveaux types.

Lorsque vous êtes prêt à redéployer, vous pouvez exécuter la même commande deploy ci-dessus. Vous pouvez visualiser la version en attente en basculant la version dans le coin supérieur droit.

---

Écrivains: [Sarah Schwartz](https://twitter.com/schwartzswartz)
Rédacteurs: [Cami](https://twitter.com/camiinthisthang)
Traducteur: [CookingCryptos](https://twitter.com/CookingCryptos)