---
title: Mise à jour de la page d'accueil
description: Let users discover events on the homepage of your full-stack decentralized event platform.
optional: false
tweet: "Build a full-stack event platform dapp with #30DaysofWeb3 @womenbuildweb3 🎫"
---

## Afficher les événements à venir

Dans notre fichier `index.js`, nous voulons pouvoir montrer tous les événements à venir auxquels les gens peuvent répondre. En haut du fichier, nous pouvons importer `gql` et `useQuery` du client Apollo. Nous aurons également besoin d'importer `useState` et notre composant `EventCard`.

```javascript
import { useState } from "react";
import { gql, useQuery } from "@apollo/client";
import EventCard from "../components/EventCard";
```

Nous pouvons définir notre requête au-dessus de notre fonction `Home` comme ceci :

```javascript
const UPCOMING_EVENTS = gql`
  query Events($currentTimestamp: String) {
    events(where: { eventTimestamp_gt: $currentTimestamp }) {
      id
      name
      eventTimestamp
      imageURL
    }
  }
`;
```

Cette requête(query) retournera l'id, le nom, l'eventTimestamp, et l'imageURL pour chaque événement qui n'a pas encore eu lieu.

Maintenant, dans notre fonction `Home`, nous pouvons récupérer le timestamp actuel et charger la requête avec le client Apollo. Une fois que nous avons obtenu la liste des événements, nous pouvons faire un mapping sur eux pour rendre une liste de cartes d'événements.

```javascript
export default function Home() {
  const [currentTimestamp, setEventTimestamp] = useState(
    new Date().getTime().toString()
  );
  const { loading, error, data } = useQuery(UPCOMING_EVENTS, {
    variables: { currentTimestamp },
  });

  if (loading)
    return (
      <Landing>
        <p>Loading...</p>
      </Landing>
    );
  if (error)
    return (
      <Landing>
        <p>`Error! ${error.message}`</p>
      </Landing>
    );

  return (
    <Landing>
      <ul
        role="list"
        className="grid grid-cols-2 gap-x-4 gap-y-8 sm:grid-cols-3 sm:gap-x-6 lg:grid-cols-4 xl:gap-x-8"
      >
        {data &&
          data.events.map((event) => (
            <li key={event.id}>
              <EventCard
                id={event.id}
                name={event.name}
                eventTimestamp={event.eventTimestamp}
                imageURL={event.imageURL}
              />
            </li>
          ))}
      </ul>
    </Landing>
  );
}
```

Maintenant, sur la page d'accueil, nous devrions être en mesure de voir une liste des événements que nous avons créés !

## ✋ Need Help?

If you need help, check to see if your question has already been asked in **#section-8-help**. If you don't see it in there, post a question with any details that would make it easy for a team member to help you. We'll answer most frequently asked questions in live office hours, so keep an eye out in **#announcements** for those!

## 🏝 C'est l'heure de la pause

Vous avez mis à jour votre page d'accueil pour afficher la liste des événements à venir - créez quelques événements et testez-les !

**Fin du 14ème jour**

---

Écrivains: [Sarah Schwartz](https://twitter.com/schwartzswartz)
Traducteur: [CookingCryptos](https://twitter.com/CookingCryptos)
