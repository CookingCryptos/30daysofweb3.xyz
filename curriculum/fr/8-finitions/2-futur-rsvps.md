---
title: RSVP à venir
description: Let users view upcoming events they RSVP'ed to on your full-stack decentralized event platform.
optional: false
tweet: "Build a full-stack event platform dapp with #30DaysofWeb3 @womenbuildweb3 🎫"
---

Dans le dossier `pages/my-rsvps`, nous avons deux pages où nous voulons montrer les événements passés et à venir auxquels l'utilisateur a répondu.

Vous pouvez ouvrir cette page à http://localhost:3000/my-rsvps/upcoming, ou vous pouvez y accéder depuis la page d'accueil en connectant votre wallet et en cliquant sur l'adresse de votre wallet dans le coin supérieur droit pour ouvrir un menu déroulant.

Dans le fichier `upcoming.js`, nous pouvons importer `useState`, `useAccount` et `ConnectButton` pour que l'utilisateur puisse connecter son wallet. Nous pouvons également importer `gql` et `useQuery` pour obtenir des détails sur l'événement à partir de notre subgraph. Enfin, nous pouvons importer le composant `EventCard`.

```javascript
import { useState } from "react";
import { gql, useQuery } from "@apollo/client";
import { ConnectButton } from "@rainbow-me/rainbowkit";
import { useAccount } from "wagmi";
import EventCard from "../../components/EventCard";
```

Avant notre fonction `MyUpcomingRSVP`, nous pouvons définir notre requête gql qui récupérera tous les rsvps pour le compte de l'utilisateur.

```javascript
const MY_UPCOMING_RSVPS = gql`
  query Account($id: String) {
    account(id: $id) {
      id
      rsvps {
        event {
          id
          name
          eventTimestamp
          imageURL
        }
      }
    }
  }
`;
```

Pour ne montrer que les rsvps des événements à venir, nous pouvons filtrer les événements retournés par la requête par le `eventTimestamp`.

Nous voulons également permettre à l'utilisateur de connecter son wallet comme nous l'avons fait sur nos autres pages avec le `ConnectButton` et le hook `useAccount`.

Nous pouvons obtenir l'adresse du wallet de l'utilisateur à partir du hook `useAccount` et la passer dans notre requête. Pour s'assurer que notre sous-graphe est capable de faire correspondre l'adresse correctement, nous devons transformer l'adresse en minuscules.

Une fois que nous avons les résultats de notre requête, nous pouvons les passer dans notre composant `EventCard`.


```javascript
export default function MyUpcomingRSVPs() {
  const { data: account } = useAccount();

  const id = account ? account.address.toLowerCase() : "";
  const [currentTimestamp, setEventTimestamp] = useState(new Date().getTime());
  const { loading, error, data } = useQuery(MY_UPCOMING_RSVPS, {
    variables: { id },
  });

  if (loading)
    return (
      <Dashboard page="rsvps" isUpcoming={true}>
        <p>Loading...</p>
      </Dashboard>
    );
  if (error)
    return (
      <Dashboard page="rsvps" isUpcoming={true}>
        <p>`Error! ${error.message}`</p>
      </Dashboard>
    );

  return (
    <Dashboard page="rsvps" isUpcoming={true}>
      {account ? (
        <div>
          {data && !data.account && <p>No upcoming RSVPs found</p>}
          {data && data.account && (
            <ul
              role="list"
              className="grid grid-cols-2 gap-x-4 gap-y-8 sm:grid-cols-3 sm:gap-x-6 lg:grid-cols-4 xl:gap-x-8"
            >
              {data.account.rsvps.map(function (rsvp) {
                if (rsvp.event.eventTimestamp > currentTimestamp) {
                  return (
                    <li key={rsvp.event.id}>
                      <EventCard
                        id={rsvp.event.id}
                        name={rsvp.event.name}
                        eventTimestamp={rsvp.event.eventTimestamp}
                        imageURL={rsvp.event.imageURL}
                      />
                    </li>
                  );
                }
              })}
            </ul>
          )}
        </div>
      ) : (
        <div className="flex flex-col items-center py-8">
          <p className="mb-4">Please connect your wallet to view your rsvps</p>
          <ConnectButton />
        </div>
      )}
    </Dashboard>
  );
}
```

## ✋ Need Help?

If you need help, check to see if your question has already been asked in **#section-8-help**. If you don't see it in there, post a question with any details that would make it easy for a team member to help you. We'll answer most frequently asked questions in live office hours, so keep an eye out in **#announcements** for those!

---

Écrivains: [Sarah Schwartz](https://twitter.com/schwartzswartz)
Traducteur: [CookingCryptos](https://twitter.com/CookingCryptos)
