---
title: Utilisation du playground The Graph
description: Test your subgraph using The Graph playground.
optional: false
tweet: "Query a subgraph for a full-stack dapp with #30DaysofWeb3 @womenbuildweb3 ⛓"
---

Maintenant que nous avons créé un événement, nous devons être en mesure de récupérer les informations relatives à l'événement pour les afficher sur la page des détails de l'événement. Nous devons également connaître le montant du dépôt pour l'événement avant de pouvoir créer un RSVP.

C'est là que notre subgraph entre en jeu. Lorsque nous ouvrons le playground de notre subgraph déployé, nous pouvons voir qu'il y a un exemple de requête par défaut dans le conteneur de gauche.

Si nous cliquons sur le bouton "play" ou "execute", nous pouvons voir que cette requête renvoie une liste de données au format JSON avec les mêmes champs que notre requête. Vous pouvez voir une liste complète des champs qui peuvent être interrogés pour chaque entité en cliquant sur le nom de l'entité dans la section Schéma à droite.

![Le playground de la requête du service hébergé du graphique](https://i.imgur.com/eYDRuF9.png)

Vous pouvez également copier et coller l'url HTTP dans une application de test d'API comme Postman ou Insomnia si vous préférez tester de cette manière.

Comme le montre l'exemple de requête, nous pouvons limiter le nombre de résultats retournés en utilisant le mot-clé `first`

```javascript
{
  events(first: 20) {
        id
  	name
  }
}
```

Si nous voulons rechercher une entrée avec une valeur spécifique pour un champ, nous pouvons le faire en définissant la valeur dans les paramètres de la requête. Par exemple, si nous disposons de l'identifiant d'une entité d'événement, nous pouvons le rechercher de la manière suivante :

```javascript
{
  event(id: "1234") {
        id
  	name
  }
}
```

Pour interroger plusieurs entités, nous pouvons utiliser le mot-clé `where`. Le mot-clé `where` est défini sur un objet avec des valeurs de recherche définies pour un certain champ de l'entité.

Si nous voulons rechercher tous les événements portant un certain nom, nous pouvons remplacer "event" par "events" et définir le champ "name" par le nom de notre événement.

```javascript
{
  events(where: { name: "Holiday Party" }) {
    id
    name
  }
}
```

Nous pouvons également attacher des modificateurs à la fin du champ pour ajouter d'autres contraintes et filtres. Par exemple, si nous voulons trouver tous les événements où le champ nom n'est pas nul, nous pouvons utiliser la requête ci-dessous :

```javascript
{
  events(where: { name_not: null }) {
    id
    name
  }
}
```

Vous pouvez consulter la liste complète des modificateurs ici : https://thegraph.com/docs/en/developer/graphql-api/#all-filters.

Nous pouvons également ordonner nos événements en utilisant le mot-clé orderBy. Pour ordonner tous les événements par le `eventTimestamp`, nous pouvons utiliser cette requête :

```javascript
{
  events(orderBy: eventTimestamp) {
    id
    name
    eventTimestamp
  }
}
```

Vous pouvez trouver une référence complète pour l'interrogation dans les docs de The Graph ici : https://thegraph.com/docs/en/developer/graphql-api/

## ✋ Need Help?

If you need help, check to see if your question has already been asked in **#section-7-help**. If you don't see it in there, post a question with any details that would make it easy for a team member to help you. We'll answer most frequently asked questions in live office hours, so keep an eye out in **#announcements** for those!

---

Écrivains: [Sarah Schwartz](https://twitter.com/schwartzswartz),
Rédacteurs: [Sarah Z](https://twitter.com/haegeez)
Traducteur: [CookingCryptos](https://twitter.com/CookingCryptos)
