---
title: Récupérer les détails de l'événement
description: Fetch event details from your subgraph.
optional: false
tweet: "I just queried my subgraph with #30DaysofWeb3 @womenbuildweb3 ⛓"
---

Ouvrez le fichier `pages/event/[id].js`, qui utilise le routage dynamique avec Next.js pour créer une nouvelle page pour chaque événement minter en fonction de l'eventID. C'est ici que nous pouvons afficher les détails d'un seul événement et que les utilisateurs peuvent y répondre.

D'abord, nous devons importer `gql` le client apollo en haut de la page.

```javascript
import { gql } from "@apollo/client";
import client from "../../apollo-client";
```

Nous pouvons utiliser la fonction `getServerSideProps` pour récupérer les données de notre subgraph depuis le serveur. Vous pouvez en apprendre plus sur cette fonction et sur la façon dont Next.js restitue le contenu ici si vous le souhaitez : https://nextjs.org/docs/basic-features/data-fetching/get-server-side-props.

Nous pouvons obtenir l'ID de l'événement à partir de l'url de la page, et le passer dans notre requête pour récupérer les détails de cet événement. Ensuite, nous pouvons renvoyer les données que nous obtenons en tant que props à utiliser sur la page.

```javascript
export async function getServerSideProps(context) {
  const { id } = context.params;

  const { data } = await client.query({
    query: gql`
      query Event($id: String!) {
        event(id: $id) {
          id
          eventID
          name
          description
          link
          eventOwner
          eventTimestamp
          maxCapacity
          deposit
          totalRSVPs
          totalConfirmedAttendees
          imageURL
          rsvps {
            id
            attendee {
              id
            }
          }
        }
      }
    `,
    variables: {
      id: id,
    },
  });

  return {
    props: {
      event: data.event,
    },
  };
}
```

Maintenant, nous pouvons importer l'événement de nos props dans la fonction Event.

```javascript
function Event({event}) {
```

Remarquez que cela ressemble beaucoup à la requête de notre playground, mais elle est intégrée dans un objet de requête appelé **Event** où nous devons définir le type d'entrée de la requête (dans ce cas, il s'agit d'une chaîne).

Maintenant, nous pouvons accéder à l'événement à partir des props en utilisant la déstructuration. Pour s'assurer que nous recevons bien les données de l'événement que nous avons demandé, nous pouvons essayer d'enregistrer l'`event` dans la console.

```javascript
function Event({ event }) {
    console.log("EVENT:", event)
```

Créez un nouvel événement sur la page de création d'événement, puis cliquez sur le lien vers la page de détails de votre événement une fois la transaction effectuée. Il se peut que vous deviez attendre quelques minutes pour que la transaction soit effectuée.

Une fois que vous pouvez ouvrir la page de détails de l'événement, vous devriez être en mesure de voir les détails de votre événement dans la console.

Nous pouvons maintenant utiliser ces données pour remplacer les valeurs statiques de la page.

## ✋ Need Help?

If you need help, check to see if your question has already been asked in **#section-7-help**. If you don't see it in there, post a question with any details that would make it easy for a team member to help you. We'll answer most frequently asked questions in live office hours, so keep an eye out in **#announcements** for those!

## 🏝 C'est l'heure de la pause

Vous avez appris à interroger votre subgraph par le biais de votre application client en utilisant Apollo Client ! Demain, vous reprendrez là où vous en étiez et apprendrez à afficher les données renvoyées par votre requête dans votre dapp.

Envoyez un tweet pour partager une mise à jour, collez le lien dans **#builders-hype** et faites du battage publicitaire pour les autres constructeurs 🔥.

**Fin du 14ème jour**

---

Écrivains: [Sarah Schwartz](https://twitter.com/schwartzswartz),
Rédacteurs: [Sarah Z](https://twitter.com/haegeez)
Traducteur: [CookingCryptos](https://twitter.com/CookingCryptos)

