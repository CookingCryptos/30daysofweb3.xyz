---
title: Événements à venir
description: Let users view upcoming events they created on your full-stack decentralized event platform.
optional: false
tweet: "About half way through the @womenbuildweb3 #30DaysOfWeb3 🎫"
---

Vous pouvez trouver la page des événements à venir dans le dossier `pages/my-events` et sur http://localhost:3000/my-events/upcoming.

En haut du fichier, nous pouvons importer à nouveau nos utilitaires d'aide.

```javascript
import { useState } from "react";
import { gql, useQuery } from "@apollo/client";
import { ConnectButton } from "@rainbow-me/rainbowkit";
import { useAccount } from "wagmi";
import EventCard from "../../components/EventCard";
```

Pour les événements à venir créés par l'utilisateur, nous voulons nous assurer que nous ne récupérons que les événements futurs où le `eventOwner` est égal à l'adresse du Wallet de l'utilisateur.

Nous pouvons le faire en combinant ces deux conditions avec le mot-clé `where` et le modificateur `_gt`.

```javascript
const MY_UPCOMING_EVENTS = gql`
  query Events($eventOwner: String, $currentTimestamp: String) {
    events(
      where: { eventOwner: $eventOwner, eventTimestamp_gt: $currentTimestamp }
    ) {
      id
      eventID
      name
      description
      eventTimestamp
      maxCapacity
      totalRSVPs
      imageURL
    }
  }
`;
```

Ensuite, nous pouvons configurer le résultat de la requête et le bouton "Connecter le portefeuille" comme nous l'avons fait sur d'autres pages et faire correspondre nos résultats aux cartes d'événements.

```javascript
export default function MyUpcomingEvents() {
  const { data: account } = useAccount();

  const eventOwner = account ? account.address.toLowerCase() : "";
  const [currentTimestamp, setEventTimestamp] = useState(
    new Date().getTime().toString()
  );
  const { loading, error, data } = useQuery(MY_UPCOMING_EVENTS, {
    variables: { eventOwner, currentTimestamp },
  });

  if (loading)
    return (
      <Dashboard page="events" isUpcoming={true}>
        <p>Loading...</p>
      </Dashboard>
    );
  if (error)
    return (
      <Dashboard page="events" isUpcoming={true}>
        <p>`Error! ${error.message}`</p>
      </Dashboard>
    );

  return (
    <Dashboard page="events" isUpcoming={true}>
      {account ? (
        <div>
          {data && data.events.length == 0 && <p>No upcoming events found</p>}
          {data && data.events.length > 0 && (
            <ul
              role="list"
              className="grid grid-cols-2 gap-x-4 gap-y-8 sm:grid-cols-3 sm:gap-x-6 lg:grid-cols-4 xl:gap-x-8"
            >
              {data.events.map((event) => (
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
          )}
        </div>
      ) : (
        <div className="flex flex-col items-center py-8">
          <p className="mb-4">Please connect your wallet to view your events</p>
          <ConnectButton />
        </div>
      )}
    </Dashboard>
  );
}
```

## ✋ Need Help?

If you need help, check to see if your question has already been asked in **#section-8-help**. If you don't see it in there, post a question with any details that would make it easy for a team member to help you. We'll answer most frequently asked questions in live office hours, so keep an eye out in **#announcements** for those!

**Fin du 15ème jour**

---

Écrivains: [Sarah Schwartz](https://twitter.com/schwartzswartz)
Traducteur: [CookingCryptos](https://twitter.com/CookingCryptos)
