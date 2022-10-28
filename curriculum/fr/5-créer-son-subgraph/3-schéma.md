---
title: Schéma
description: Define the GraphQL schema that models the data you will query from your subgraph.
optional: false
tweet: "I just defined my subgraph schema on @graphprotocol with #30DaysofWeb3 @womenbuildweb3 👾"
---

C'est ici que nous allons définir le schéma (schema.graphql) pour nos requêtes GraphQL. Idéalement, nous voulons modéliser ces données pour qu'elles correspondent aux données dont nous avons besoin pour notre frontend.

**Sur la base de notre projet de site web, nous pouvons voir que nous devons être en mesure de:**

- rechercher des événements par titre, description
- filtrer les événements par date et/ou intervalle de dates
- récupérer les événements hébergés par l'utilisateur actuel par date (à venir ou passés)
- récupérer les événements rsvp de l'utilisateur actuel par date (à venir ou passés)
- trier les événements par nombre maximum de rsvps
- trier les événements par date
- Vérifier si l'utilisateur actuel a répondu à l'événement.
- Vérifier si l'utilisateur actuel a participé à l'événement
- Récupérer les détails de l'événement

Nous pouvons supprimer l'entité exemple dans le fichier de schéma. Voici à quoi devrait ressembler notre fichier de schéma :

```graphql
type Event @entity {
  id: ID!
  eventID: Bytes!
  name: String
  description: String
  link: String
  imageURL: String
  eventOwner: Bytes!
  eventTimestamp: BigInt!
  maxCapacity: BigInt!
  deposit: BigInt!
  paidOut: Boolean!
  totalRSVPs: BigInt!
  totalConfirmedAttendees: BigInt!
  rsvps: [RSVP!] @derivedFrom(field: "event")
  confirmedAttendees: [Confirmation!] @derivedFrom(field: "event")
}

type Account @entity {
  id: ID!
  totalRSVPs: BigInt!
  totalAttendedEvents: BigInt!
  rsvps: [RSVP!] @derivedFrom(field: "attendee")
  attendedEvents: [Confirmation!] @derivedFrom(field: "attendee")
}

type RSVP @entity {
  id: ID!
  attendee: Account!
  event: Event!
}

type Confirmation @entity {
  id: ID!
  attendee: Account!
  event: Event!
}
```

Décomposons ce qui se passe ici.

Nous avons 4 entités : `Event`, `Account`, `RSVP`, et `Confirmation`. Chaque entité doit être définie avec la balise `@entity`. Vous pouvez considérer une entité comme un objet. C'est une "chose" qui a des clés et des valeurs, chaque clé étant un champ de schéma qui peut être interrogé pour connaître sa valeur.

Chaque entité possède également un champ ID pour un identifiant unique, ainsi que des champs contenant des informations sur l'entité que l'on souhaite pouvoir interroger. Chaque champ a un type, et s'il est obligatoire, il est accompagné d'un " !" (la valeur ne peut pas être nulle). Les champs nom de l'événement, description, lien et imageURL ne sont pas obligatoires, ce qui signifie qu'ils peuvent renvoyer une valeur `nulle`.

Dans les entités `Event` et `Account`, pour les champs rsvps et confirmedAttendees, nous utilisons un mot-clé spécial `@derivedFrom`, également appelé reverse lookup, qui nous permet de référencer les données d'une autre entité. Prenons l'exemple du champ rsvps :

```graphql
rsvps: [RSVP!] @derivedFrom(field: "event")
```

Une façon de penser à ce qui se passe ici est que chaque fois que quelqu'un crée un nouveau RSVP, cette instance RSVP est ajoutée à ce tableau si l'identifiant de l'événement du champ "event" de l'entité RSVP correspond à l'identifiant de cet événement. Par exemple, une nouvelle RSVP est créée :

```graphql
RSVP {
  id: 20
  attendee: 300
  event: 5000
}
```

Cette RSVP concerne un événement dont l'identifiant est 5000. L'événement avec l'id correspondant aura cette RSVP ajoutée au tableau rsvps.

```graphql
Event {
  id: 5000
  rsvps: [ {id: 20, attendee: 300, event: 5000} ]
}
```

Chaque nouvelle RSVP pour cet événement sera également ajoutée ici. En utilisant des recherches inversées comme celle-ci, nous sommes facilement en mesure d'interroger toutes les RSVP et les confirmations pour tout événement ou compte.

## ✋ Need Help?

If you need help, check to see if your question has already been asked in **#section-5-help**. If you don't see it in there, post a question with any details that would make it easy for a team member to help you. We'll answer most frequently asked questions in live office hours, so keep an eye out in **#announcements** for those!

## 🏝 C'est l'heure de la pause

Vous avez créé le support pour votre sous-graphe et défini votre schéma. Vous connaissez l'exercice - documentez votre voyage en appuyant sur le bouton ci-dessous pour tweeter à ce sujet, collez votre tweet dans **#builders-hype** et faites du bruit avec les autres tweets que vous voyez publiés.

**Fin du 7ème jour**

---

Écrivains: [Sarah Schwartz](https://twitter.com/schwartzswartz)
Rédacteurs: [Cami](https://twitter.com/camiinthisthang)
Traducteur: [CookingCryptos](https://twitter.com/CookingCryptos)
