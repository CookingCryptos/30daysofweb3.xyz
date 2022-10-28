---
title: Configuration de The Graph
description: Setup your subgraph on the hosted service.
optional: false
tweet: "Create and deploy a subgraph on @graphprotocol with #30DaysofWeb3 @womenbuildweb3 👾"
---

Ensuite, nous allons créer un subgraph sur le service hébergé où nous pourrons déployer notre code. Allez sur [thegraph.com] (https://thegraph.com/) et sélectionnez le service hébergé dans le menu déroulant des produits. Vous devrez vous connecter avec votre compte Github.

Accédez à l'onglet " My Dashboard ", puis cliquez sur le bouton " Add a Subgraph ".

Remplissez les champs obligatoires comme vous le souhaitez, puis cliquez sur le bouton " Create subgraph " en bas.
Remarque : vous pouvez modifier ces informations ultérieurement en cliquant sur l'icône en forme de crayon sur la page de votre sous-graphe.

Naviguez jusqu'à l'endroit où vous souhaitez enregistrer votre subgraph dans votre terminal et collez la commande ci-dessous. Veillez à remplacer l'utilisateur Github et le nom du subgraph par les vôtres. (Astuce : vous pouvez trouver ceci à la fin de l'url de votre subgraph)

```
graph init --produit hosted-service <GITHUB_USER>/<SUBGRAPH NAME>
```

Vous allez maintenant être invité à choisir plusieurs options pour votre subgraph.

Pour le protocole, nous choisirons Ethereum. Vous pouvez changer le nom du subgraph et du répertoire si vous le souhaitez, ou vous pouvez simplement appuyer sur entrée.

Pour le réseau, _nous choisirons Mumbai_.

Pour l'adresse du contrat, entrez l'adresse du contrat que vous avez déployé.

Pour le nom du contrat, nous utiliserons Web3RSVP.

**Note** : vous ne pouvez pas avoir d'espaces ou de tirets ici, ou la configuration échouera.

Ensuite, il vous sera demandé si vous souhaitez ajouter un autre contrat. Assurez-vous que cette option est réglée sur **false** et appuyez sur Entrée.

---

Écrivains: [Sarah Schwartz](https://twitter.com/schwartzswartz),
Rédacteurs: [Kristen](https://twitter.com/cuddleofdeath), [Cami](https://twitter.com/camiinthisthang)
Traducteur: [CookingCryptos](https://twitter.com/CookingCryptos)
