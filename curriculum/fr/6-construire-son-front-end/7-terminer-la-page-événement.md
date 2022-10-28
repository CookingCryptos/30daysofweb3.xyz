---
title: Terminer la page de création d'un événement
description: Let users create events on your full-stack decentralized event platform.
optional: false
tweet: "Build a full-stack event platform dapp with #30DaysofWeb3 @womenbuildweb3 💥"
---

En haut de la page `create-event`, importez le `connectButton` de RainbowKit, `useAccount` de wagmi, et le composant `Alert`.

```javascript
import { ConnectButton } from "@rainbow-me/rainbowkit";
import { useAccount } from "wagmi";
import Alert from "../components/Alert";
```

En haut de `CreateEvent`, juste sous `export default function CreateEvent() {`, configurez la variable `account` avec `useAccount` :

```javascript
const { data: account } = useAccount();
```

Nous allons également mettre en place des variables `state` pour garder la trace de nos messages d'alerte, afin que nos utilisateurs puissent voir si leur événement a été créé avec succès ou non. Ajoutez-les juste en dessous de la ligne `useAccount()` ci-dessus :

```javascript
const [success, setSuccess] = useState(null);
const [message, setMessage] = useState(null);
const [loading, setLoading] = useState(null);
const [eventID, setEventID] = useState(null);
```

Dans notre fonction `createEvent`, juste avant que la console n'enregistre "Minting..." et le hash de la transaction, nous pouvons mettre le statut de `loading` à true. Une fois que la transaction s'est déroulée avec succès, nous pouvons définir notre variable de succès à true, définir `loading` à false, et définir notre message de succès.

```javascript
setLoading(true);
console.log("Minting...", txn.hash);
let wait = await txn.wait();
console.log("Minted -- ", txn.hash);
setEventID(wait.events[0].args[0]);
setSuccess(true);
setLoading(false);
setMessage("Your event has been created successfully.");
```

Si nous rencontrons une erreur, nous pouvons définir le message pour montrer l'erreur.

```javascript
setSuccess(false);
setMessage(`There was an error creating your event: ${error.message}`);
setLoading(false);
```

Voici à quoi devrait ressembler votre fonction `createEvent` maintenant :

```javascript
const createEvent = async (cid) => {
  try {
    const rsvpContract = connectContract();

    if (rsvpContract) {
      let deposit = ethers.utils.parseEther(refund);
      let eventDateAndTime = new Date(`${eventDate} ${eventTime}`);
      let eventTimestamp = eventDateAndTime.getTime();
      let eventDataCID = cid;

      const txn = await rsvpContract.createNewEvent(
        eventTimestamp,
        deposit,
        maxCapacity,
        eventDataCID,
        { gasLimit: 900000 }
      );
      setLoading(true);
      console.log("Minting...", txn.hash);
      let wait = await txn.wait();
      console.log("Minted -- ", txn.hash);

      setEventID(wait.events[0].args[0]);

      setSuccess(true);
      setLoading(false);
      setMessage("Your event has been created successfully.");
    } else {
      console.log("Error getting contract.");
    }
  } catch (error) {
    setSuccess(false);
    setMessage(`There was an error creating your event: ${error.message}`);
    setLoading(false);
    console.log(error);
  }
};
```

Maintenant, nous pouvons configurer le composant d'alerte à afficher en fonction de l'état de réussite et de chargement. Nous pouvons l'ajouter à l'intérieur de la `section className="relative py-12`.

```javascript
{
  loading && (
    <Alert
      alertType={"loading"}
      alertBody={"Please wait"}
      triggerAlert={true}
      color={"white"}
    />
  );
}
{
  success && (
    <Alert
      alertType={"success"}
      alertBody={message}
      triggerAlert={true}
      color={"palegreen"}
    />
  );
}
{
  success === false && (
    <Alert
      alertType={"failed"}
      alertBody={message}
      triggerAlert={true}
      color={"palevioletred"}
    />
  );
}
```

Nous pouvons également intégrer notre formulaire et notre en-tête dans une déclaration conditionnelle afin qu'ils ne s'affichent pas si l'utilisateur réussit à créer un événement.

```javascript
{
  !success && (
    <h1 className="text-3xl tracking-tight font-extrabold text-gray-900 sm:text-4xl md:text-5xl mb-4">
      Create your virtual event
    </h1>
  );
}
```

Nous pouvons également masquer le formulaire si un utilisateur n'a pas connecté son wallet.

```javascript
{
  account && !success && <form>...</form>;
}
```

Passez ensuite à la section commentée demandant à l'utilisateur de connecter son wallet, décommentez-la pour ne l'afficher que si l'utilisateur n'a pas encore connecté son wallet.

```javascript
{
  !account && (
    <section className="flex flex-col items-start py-8">
      <p className="mb-4">Please connect your wallet to create events.</p>
      <ConnectButton />
    </section>
  );
}
```

Si l'événement est créé avec succès, nous pouvons montrer à l'utilisateur un message de succès et un lien vers la page de son événement. Nous pouvons ajouter ceci au bas de la `section`.

```javascript
{
  success && eventID && (
    <div>
      Success! Please wait a few minutes, then check out your event page{" "}
      <span className="font-bold">
        <Link href={`/event/${eventID}`}>here</Link>
      </span>
    </div>
  );
}
```

Et c'est tout ! Testez la page pour voir si vous êtes en mesure de créer un nouvel événement.

> Note : si vous parvenez à créer un événement, vous recevrez un message de confirmation contenant un lien vers la page de l'événement. Vous travaillerez sur la configuration des pages d'événements dans la section 7. En avant !

Si vous rencontrez des erreurs, vous pouvez voir une copie complète de cette page ici : https://github.com/womenbuildweb3/Web3RSVP-frontend/blob/main/pages/create-event.js

---

Écrivains: [Sarah Schwartz](https://twitter.com/schwartzswartz),
Rédacteurs: [Sarah Z](https://twitter.com/haegeez), [Jordan Rob](https://twitter.com/Jordan___Rob)
Traducteur: [CookingCryptos](https://twitter.com/CookingCryptos)

