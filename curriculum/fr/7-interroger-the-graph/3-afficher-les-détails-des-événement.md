---
title: Affichage des détails d'un événement
description: Display the event details you fetched from your subgraph.
optional: false
tweet: "Query a subgraph for a full-stack dapp with #30DaysofWeb3 @womenbuildweb3 ⛓"
---

Dans la section `Head`, nous pouvons changer "name" dans la balise `<title>` et dans le contenu `meta` en `{event.name}`.

```javascript
<Head>
  <title> {event.name} | web3rsvp</title>
  <meta name="description" content={event.name} />
  <link rel="icon" href="/favicon.ico" />
</Head>
```

Ensuite, nous allons importer une fonction pour formater le timestamp appelée `formatTimestamp`. Cette fonction est tirée de notre dossier utils.

Importez ce qui suit quelque part en haut de votre fichier avec toutes les autres importations :

```javascript
import formatTimestamp from "../../utils/formatTimestamp";
```

Et remplacez le texte qui dit "time" par l'heure formatée.

```javascript
<h6 className="mb-2">{formatTimestamp(event.eventTimestamp)}</h6>
```

Nous pouvons également remplacer les autres détails de l'événement. Ainsi, `name` peut être changé en `{event.name}`, et `description` peut être changé en `{event.description}`.

Nous pouvons changer `# attending` pour afficher le total des RSVP et la capacité maximale avec `{event.totalRSVP}/{event.maxCapacity} attending` .

Pour afficher l'image de l'événement, nous devrons importer le composant `Image` de `next/image` en haut du fichier.

```javascript
import Image from "next/image";
```

Nous pouvons maintenant ajouter l'image au-dessus de la description de l'événement. Nous n'afficherons l'image que si l'imageURL n'est pas nulle.

```javascript
<div className="mb-8 w-full aspect-w-10 aspect-h-7 rounded-lg bg-gray-100 focus-within:ring-2 focus-within:ring-offset-2 focus-within:ring-offset-gray-100 focus-within:ring-indigo-500 overflow-hidden">
  {event.imageURL && (
    <Image src={event.imageURL} alt="event image" layout="fill" />
  )}
</div>
<p>{event.description}</p>
```

Après `Hosted by{" "}`, à l'intérieur de la balise `<a>`, nous pouvons ajouter l'adresse du propriétaire de l'événement avec `{event.eventOwner}`. Ensuite, nous pouvons lier l'adresse à l'explorateur testnet en utilisant notre variable `NEXT_PUBLIC_TESTNET_EXPLORER_URL`.

```javascript
<span className="truncate">
  Hosted by{" "}
  <a
    className="text-indigo-800 truncate hover:underline"
    href={`${process.env.NEXT_PUBLIC_TESTNET_EXPLORER_URL}address/${event.eventOwner}`}
    target="_blank"
    rel="noreferrer"
  >
    {event.eventOwner}
  </a>
</span>
```

Vous devriez maintenant être en mesure de voir tous les détails de l'événement !

---

Écrivains: [Sarah Schwartz](https://twitter.com/schwartzswartz),
Rédacteurs: [Sarah Z](https://twitter.com/haegeez)
Traducteur: [CookingCryptos](https://twitter.com/CookingCryptos)
