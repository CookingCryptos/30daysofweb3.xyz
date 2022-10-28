---
title: Événements passés
description: Let users view past events they created on your full-stack decentralized event platform.
optional: false
tweet: "Build a full-stack event platform dapp with #30DaysofWeb3 @womenbuildweb3 🎫"
---

Dans le dossier `pages/my-events/past`, ouvrez le fichier `index.js`.

En haut du fichier, nous pouvons à nouveau importer nos utilitaires d'aide.

```javascript
import { useState } from "react";
import Link from "next/link";
import { gql, useQuery } from "@apollo/client";
import { ConnectButton } from "@rainbow-me/rainbowkit";
import { useAccount } from "wagmi";
import EventCard from "../../../components/EventCard";
```

Nous allons définir notre requête presque de la même manière que pour les événements à venir, mais au lieu du modificateur `_gt`, nous allons utiliser le modificateur `_lt` pour récupérer les événements passés

```javascript
const MY_PAST_EVENTS = gql`
  query Events($eventOwner: String, $currentTimestamp: String) {
    events(
      where: { eventOwner: $eventOwner, eventTimestamp_lt: $currentTimestamp }
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

Maintenant nous pouvons montrer tous les événements passés créés par l'utilisateur et un lien où l'utilisateur peut confirmer les participants.

```javascript
export default function MyPastEvents() {
  const { data: account } = useAccount();

  const eventOwner = account ? account.address.toLowerCase() : "";
  const [currentTimestamp, setEventTimestamp] = useState(
    new Date().getTime().toString()
  );
  const { loading, error, data } = useQuery(MY_PAST_EVENTS, {
    variables: { eventOwner, currentTimestamp },
  });

  if (loading)
    return (
      <Dashboard page="events" isUpcoming={false}>
        <p>Loading...</p>
      </Dashboard>
    );
  if (error)
    return (
      <Dashboard page="events" isUpcoming={false}>
        <p>`Error! ${error.message}`</p>
      </Dashboard>
    );

  return (
    <Dashboard page="events" isUpcoming={false}>
      {account ? (
        <div>
          {data && data.events.length == 0 && <p>No past events found</p>}
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
                  <Link href={`/my-events/past/${event.id}`}>
                    <a className="text-indigo-800 text-sm truncate hover:underline">
                      Confirm attendees
                    </a>
                  </Link>
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

---

Écrivains: [Sarah Schwartz](https://twitter.com/schwartzswartz)
Traducteur: [CookingCryptos](https://twitter.com/CookingCryptos)
