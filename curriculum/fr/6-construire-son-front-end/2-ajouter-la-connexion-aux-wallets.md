---
title: Ajouter la connexion au Wallets
description: Configure RainbowKit, wagmi, and ethers to add wallet connection to your dapp.
optional: false
tweet: "Add wallet connection to my dapp with @rainbowdotme 🌈 #30DaysofWeb3 @womenbuildweb3 🌈"
---

Maintenant que nous avons enveloppé notre application avec les composants `WagmiConfig` et `RainbowKitProvider`, nous pouvons utiliser les hooks wagmi et le composant `ConnectButton` de RainbowKit pour permettre aux utilisateurs de connecter leur Wallet et pour informer l'utilisateur que son Wallet est connecté.

Dans le fichier `/components/Navbar.js`, nous pouvons importer le composant `ConnectButton` de RainbowKit et les hooks `useAccount` et `useDisconnect` de wagmi. Ajoutez les importations suivantes sous la ligne `import Navmenu from "./Navmenu";` :

```javascript
import { ConnectButton } from "@rainbow-me/rainbowkit";
import { useAccount, useDisconnect } from "wagmi";
```

Nous utiliserons le hook `useAccount` pour accéder au Wallet connecté s'il existe, et le hook `useDisconnect` pour déconnecter le Wallet actuellement connecté. Vous pouvez ajouter ces constantes directement sous le début de notre fonction Navbar, comme suit :

```javascript
export default function Navbar() {
    const { data: account } = useAccount();
    const { disconnect } = useDisconnect();
```

Dans notre `Navbar`, nous pouvons vérifier l'état de connexion du Wallet de l'utilisateur. Si le portefeuille de l'utilisateur est connecté, nous afficherons un bouton qui indique l'adresse du wallet de l'utilisateur et fait basculer un menu déroulant. Dans le cas contraire, si le wallet de l'utilisateur n'est pas connecté, nous afficherons le bouton "Connect Wallet" de RainbowKit. Nous pouvons ajouter ce bouton après le bouton "Create Event".

```javascript
    </Link>
    {account ? (
        <Navmenu account={account} disconnect={() => disconnect()} />
    ) : (
        <ConnectButton />
    )}
</div>
```

Voici à quoi devrait ressembler votre fichier Navbar.js :

```javascript
import { useState, useEffect } from "react";
import Link from "next/link";
import Navmenu from "./Navmenu";
import { ConnectButton } from "@rainbow-me/rainbowkit";
import { useAccount, useDisconnect } from "wagmi";

export default function Navbar() {
  const { data: account } = useAccount();
  const { disconnect } = useDisconnect();
  const [mounted, setMounted] = useState(false);

  useEffect(() => {
    setMounted(true);
  }, []);

  return (
    mounted && (
      <header className="bg-white border-b-2 border-gray-100">
        <nav
          className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8"
          aria-label="Top"
        >
          <div className="w-full py-6 flex flex-wrap items-center justify-between border-b border-indigo-500 lg:border-none">
            <div className="flex items-center">
              <Link href="/">
                <a>web3rsvp</a>
              </Link>
            </div>
            <div className="ml-10 space-x-4 flex items-center">
              <Link href="/create-event">
                <a className="inline-flex items-center px-4 py-2 border border-transparent text-sm font-medium rounded-md text-indigo-700 border border-indigo-100 hover:bg-indigo-50 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
                  Create Event
                </a>
              </Link>
              {account ? (
                <Navmenu account={account} disconnect={() => disconnect()} />
              ) : (
                <ConnectButton />
              )}
            </div>
          </div>
        </nav>
      </header>
    )
  );
}
```

Nous transmettons l'objet de compte et la fonction de déconnexion à notre composant Navmenu. Les deux étapes suivantes **sont déjà faites pour vous.**

Dans le fichier `/components/Navmenu.js`, vous remarquerez que nous affichons l'adresse du wallet de connexion avec cette ligne :

```javascript
<p className="text-ellipsis overflow-hidden">{account.address}</p>
```

Nous permettons également aux utilisateurs de déconnecter leurs wallets :

```javascript
<a
  onClick={disconnect}
  className={joinClassNames(
    account ? "bg-gray-100 text-gray-900" : "text-gray-700",
    "block px-4 py-2 text-sm cursor-pointer"
  )}
>
  Log Out
</a>
```

Dans notre dApp, nous vérifions la connexion du Wallet de l'utilisateur pour afficher conditionnellement l'interface utilisateur en utilisant le hook `useAccount` de wagmi.

**Note:** RainbowKit a récemment annoncé un nouveau script en ligne de commande pour créer une application Next.js avec RainbowKit et wagmi configurés pour vous. Pour en savoir plus, cliquez ici : https://www.rainbowkit.com/docs/installation#quick-start

## ✋ Need Help?

If you need help, check to see if your question has already been asked in **#section-6-help**. If you don't see it in there, post a question with any details that would make it easy for a team member to help you. We'll answer most frequently asked questions in live office hours, so keep an eye out in **#announcements** for those!

## 🏝 C'est l'heure de la pause

Vous venez de mettre en place le boilerplate pour votre application client et de créer une connexion Wallet à l'aide de Rainbowkit 🌈 C'est le bon moment pour faire une pause. Mettez à jour le Twitterverse en appuyant sur le bouton Partager ci-dessous, collez votre tweet dans **#builders-hype** et faites du bruit pour les autres constructeurs 🔥.

**Fin du 9ème jour**

---

Écrivains: [Sarah Z](https://twitter.com/haegeez), [Busayo](https://twitter.com/AmoweO),
Traducteur: [CookingCryptos](https://twitter.com/CookingCryptos)