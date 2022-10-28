---
title: RSVP à un événement
description: Let users RSVP to an event on your full-stack decentralized event platform.
optional: false
tweet: "I've coded in 3 languages so far while building my fullstack dapp- Solidity, Assemblyscript, and Javascript 💻 #30DaysofWeb3 @womenbuildweb3 🎫"
---

## ✋ Need Help?

If you need help, check to see if your question has already been asked in **#section-8-help**. If you don't see it in there, post a question with any details that would make it easy for a team member to help you. We'll answer most frequently asked questions in live office hours, so keep an eye out in **#announcements** for those!

## RSVP aux événements

Nous voulons que les utilisateurs puissent également être en mesure de répondre à un événement sur la page de détails de l'événement.

Dans le même fichier `pages/events/[id].js`, importez notre wallet et nos fonctions de contrat en haut :

```javascript
import { useState } from "react";
import { ethers } from "ethers";
import { ConnectButton } from "@rainbow-me/rainbowkit";
import { useAccount } from "wagmi";
import connectContract from "../../utils/connectContract";
import Alert from "../../components/Alert";
```

En haut de la fonction Event, nous pouvons ajouter quelques variables d'état pour garder la trace du compte utilisateur, de l'état de la transaction du contrat et de l'heure actuelle.

```javascript
const { data: account } = useAccount();
const [success, setSuccess] = useState(null);
const [message, setMessage] = useState(null);
const [loading, setLoading] = useState(null);
const [currentTimestamp, setEventTimestamp] = useState(new Date().getTime());
```

Maintenant nous allons vérifier si l'utilisateur a déjà répondu ou non en créant une fonction appelée `checkIfAlreadyRSVPed`. S'il ne l'a pas déjà fait, l'utilisateur verra un bouton pour RSVP. Pour savoir s'il a déjà répondu, nous pouvons parcourir le tableau des réponses de l'événement et voir si l'une des adresses de portefeuille correspond.

```javascript
function checkIfAlreadyRSVPed() {
  if (account) {
    for (let i = 0; i < event.rsvps.length; i++) {
      const thisAccount = account.address.toLowerCase();
      if (event.rsvps[i].attendee.id.toLowerCase() == thisAccount) {
        return true;
      }
    }
  }
  return false;
}
```

Ensuite, nous pouvons créer une fonction appelée `newRSVP` et appeler la méthode `createNewRSVP` de notre contrat. Nous pouvons passer le montant du dépôt que nous avons récupéré de notre subgraph comme valeur de la transaction.

```javascript
const newRSVP = async () => {
  try {
    const rsvpContract = connectContract();
    if (rsvpContract) {
      const txn = await rsvpContract.createNewRSVP(event.id, {
        value: event.deposit,
        gasLimit: 300000,
      });
      setLoading(true);
      console.log("Minting...", txn.hash);

      await txn.wait();
      console.log("Minted -- ", txn.hash);
      setSuccess(true);
      setLoading(false);
      setMessage("Your RSVP has been created successfully.");
    } else {
      console.log("Error getting contract.");
    }
  } catch (error) {
    setSuccess(false);
    setMessage("Error!");
    setLoading(false);
    console.log(error);
  }
};
```

Tout comme dans notre page `create-event`, nous voulons montrer une alerte basée sur le statut de la transaction du contrat de l'utilisateur. Nous pouvons l'ajouter dans la première section de la page.

```javascript
<section className="relative py-12">
  {loading && (
    <Alert
      alertType={"loading"}
      alertBody={"Please wait"}
      triggerAlert={true}
      color={"white"}
    />
  )}
  {success && (
    <Alert
      alertType={"success"}
      alertBody={message}
      triggerAlert={true}
      color={"palegreen"}
    />
  )}
  {success === false && (
    <Alert
      alertType={"failed"}
      alertBody={message}
      triggerAlert={true}
      color={"palevioletred"}
    />
  )}
```

Au-dessus de la section qui montre le nombre de RSVP et la capacité maximale pour l'événement, nous pouvons ajouter un bouton pour RSVP que nous ne montrerons que si l'utilisateur n'a pas déjà RSVP. S'il l'a déjà fait, nous pouvons lui montrer un lien vers l'événement.

Tout ceci est enveloppé dans une déclaration conditionnelle qui vérifie également si l'utilisateur est connecté. S'il n'est pas connecté, nous pouvons lui montrer le bouton "Connect Wallet".

Si l'événement est déjà passé, nous masquerons tout cela et ferons savoir à l'utilisateur que l'événement a déjà eu lieu.

```javascript
<div className="max-w-xs w-full flex flex-col gap-4 mb-6 lg:mb-0">
  {event.eventTimestamp > currentTimestamp ? (
    account ? (
      checkIfAlreadyRSVPed() ? (
        <>
          <span className="w-full text-center px-6 py-3 text-base font-medium rounded-full text-teal-800 bg-teal-100">
            You have RSVPed! 🙌
          </span>
          <div className="flex item-center">
            <LinkIcon className="w-6 mr-2 text-indigo-800" />
            <a
              className="text-indigo-800 truncate hover:underline"
              href={event.link}
            >
              {event.link}
            </a>
          </div>
        </>
      ) : (
        <button
          type="button"
          className="w-full items-center px-6 py-3 border border-transparent text-base font-medium rounded-full text-indigo-700 bg-indigo-100 hover:bg-indigo-200 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
          onClick={newRSVP}
        >
          RSVP for {ethers.utils.formatEther(event.deposit)} MATIC
        </button>
      )
    ) : (
      <ConnectButton />
    )
  ) : (
    <span className="w-full text-center px-6 py-3 text-base font-medium rounded-full border-2 border-gray-200">
      Event has ended
    </span>
  )}
  <div className="flex item-center">
```

Et voilà ! La création du RSVP est terminée ! 🎉

Testez le bouton RSVP pour vous assurer que tout fonctionne. Cela peut prendre quelques minutes pour que la page de l'événement indique que vous avez déjà répondu.

## Builders Hype

Prenez une seconde pour faire une pause et vous imprégner de tout cela - vous avez presque terminé de construire une application complète. Vous avez créé votre Smart Contract, votre subgraph, et maintenant votre frontend. Vous avez écrit du code en 3 languages et ce n'est pas un mince exploit. Envoyez un tweet et faites savoir au monde ce que vous avez accompli 🔥

---

Écrivains: [Sarah Schwartz](https://twitter.com/schwartzswartz),
Rédacteurs: [Kristen](https://twitter.com/CuddleofDeath)
Traducteur: [CookingCryptos](https://twitter.com/CookingCryptos)
