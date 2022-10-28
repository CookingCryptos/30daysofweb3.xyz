---
title: Configurer votre environnement de développement
description: Setup your developer environment for building full-stack dapps.
optional: false
tweet: "Ship a full-stack event platform dapp with #30DaysofWeb3 @womenbuildweb3 🎫"
---

![Dev Setup](https://user-images.githubusercontent.com/15064710/180662024-6fa3c838-c8c4-4586-8d84-33ad0aa4a9f1.png)

## ✋ Need Help?

If you need help, check to see if your question has already been asked in **#section-3-help**. If you don't see it in there, post a question with any details that would make it easy for a team member to help you. We'll answer most frequently asked questions in live office hours, so keep an eye out in **#announcements** for those!

## IDE

Bien que n'importe quel IDE (Integrated Developer Environment) fonctionne, nous recommandons l'utilisation de **VSCode**, un IDE gratuit que vous pouvez télécharger [ici.](https://code.visualstudio.com/download)

## Node.js et NPM

Vous devez installer Node.js et npm sur votre machine.

**Node.js** est un environnement d'exécution qui exécute JavaScript en dehors du navigateur, permettant aux développeurs de créer des applications JavaScript complètes. **NPM** signifie *node package manager* et est l'interface en ligne de commande d'un écosystème dynamique de paquets Node.js open-source. Si vous voulez en savoir plus sur NPM, consultez [cet article](https://nodesource.com/blog/an-absolute-beginners-guide-to-using-npm/).

Pour vérifier si vous avez Node.js et npm installés, vous pouvez lancer `node -v` et `npm -v`. Si vous obtenez un nombre qui ressemble à 8.0.6, cela signifie que vous les avez installés.

![node version](https://user-images.githubusercontent.com/15346823/179375406-e00e704f-0dfe-40a4-82a3-82463766fe4c.png)

Pour télécharger Node.js et npm, nous vous recommandons d'utiliser **nvm**, un gestionnaire de version de node qui vous aide à gérer les différentes versions de Node.js et npm. Suivez le guide d'installation [ici](https://github.com/nvm-sh/nvm#installing-and-updating)

Une fois que vous avez installé nvm, exécutez ce qui suit pour télécharger une version stable de Node.js et npm.

```
nvm install --lts
nvm use --lts
```

Confirmez votre version de Node.js en appelant `nvm current`. Vous devriez utiliser Node v16.

Maintenant vous êtes prêt à partir ! Nous installerons les dépendances dans notre application au fur et à mesure que nous la construirons, mais ceci vous permettra d'être capable de télécharger en douceur.

## Git et Github

Si vous voulez recevoir des félicitations pour avoir complété les points de contrôle et soumis votre projet et/ou si vous allez postuler pour notre BUIDL Accelerator, vous devrez soumettre un lien vers un dépôt git comme preuve de travail.

Si vous n'avez pas l'habitude d'utiliser git ou GitHub, vous pouvez commencer avec ce [tutoriel ici](https://www.youtube.com/watch?v=8Dd7KRpKeaE).

## Configuration du projet

### Créer un Smart Contract

Pendant ces 30 jours, nous allons travailler à la création d'une application RSVP complète. Pensez-y comme un eventbrite natif de web3, sauf que les participants doivent déposer de l'ETH pour se réinscrire et qu'ils le récupèrent lorsqu'ils se présentent à l'événement.

Commençons à coder ! Aujourd'hui, nous allons écrire environ ½ du Smart Contract.

**Créer un nouveau projet depuis votre terminal:**

- Naviguez vers le dossier dans lequel vous voulez créer ce projet. Si vous voulez créer ce projet sur votre bureau, naviguez vers ce répertoire en exécutant `cd Desktop`.
- Créez un nouveau dossier pour votre projet `mkdir web3rsvp`.
- N'oubliez pas de naviguer vers le nouveau répertoire du projet en utilisant `cd web3rsvp` ou vos fichiers se retrouveront sur votre bureau et non dans le dossier.
- Exécutez la commande suivante pour initialiser un projet npm : `npm init` et suivez ensuite les instructions (vous pouvez appuyer sur entrée et dire oui à tout si vous le souhaitez !)
- Après l'initialisation du projet et la création réussie d'un fichier `package.json`, exécutez la commande suivante pour installer hardhat (_un environnement de développement pour compiler, déployer, tester et déboguer votre code Ethereum_) : `npm install --save-dev hardhat` ou `yarn add --dev hardhat`.
- Après cela, exécutez `npx hardhat`.
- Sélectionnez un projet de base, sélectionnez `y` pour toutes les invites. `y` signifie simplement `yes`.
- Enfin, ouvrez ce projet nouvellement créé dans votre éditeur de code. Faites-le en ouvrant VS code et en appuyant sur `file` > `open` > cherchez le dossier que vous venez de créer.

> Note : Si vous avez des problèmes avec cette configuration, suivez les étapes ci-dessous pour forker et cloner le repo de démarrage.

**Fork et cloner le repo de démarrage::**

Tout d'abord, allez sur le [starter repo](https://github.com/womenbuildweb3/hardhat-sample) et fork le projet:

![fork le projet](https://user-images.githubusercontent.com/56278409/183780829-826e0ed4-1e01-4678-8d2e-9fe54bd8913d.png)

Ensuite, clonez le repo sur votre machine en copiant le lien qui vous est donné lorsque vous appuyez sur `code.` Notez que le nom d'utilisateur dans le lien doit être votre nom d'utilisateur GitHub. Si vous voyez toujours `womenbuildweb3` comme nom d'utilisateur, vous n'avez pas forké le repo. Après avoir copié le lien, exécutez cette commande dans votre terminal :
`git clone <insert link here>`

![code](https://user-images.githubusercontent.com/56278409/183777859-a5bcbf8f-a641-4ad4-9181-1b347ecd34a3.png)

Dans le dossier `contracts`, allez à votre fichier `Greeter.sol` et renommez-le en `Web3RSVP.sol`.

> Note: Greeter.sol a été mis à jour en Lock.sol

Supprimez tout ce qui se trouve dans ce fichier à l'exception des deux premières lignes, plus le commentaire tout en haut.

Voici à quoi devrait ressembler votre fichier :

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;
```

Le SPDX-License-Identifier permet de définir la licence de notre fichier. Dans ce projet, nous avons choisi la [MIT license](https://spdx.org/licenses/MIT.html). C'est parce que notre projet est entièrement open source.

La ligne `pragma solidity ^0.8.4;` indique au compilateur quelle version de Solidity il doit utiliser lors de la compilation. **Note importante:** Il est nécessaire que la ligne pragma solidity corresponde à la version de Solidity définie dans le module exports de votre fichier hardhat.config.js.

---

Écrivain: [Cami](https://twitter.com/camiinthisthang),
Rédacteurs: [Sarah Z](https://twitter.com/haegeez), [Kristen](https://twitter.com/cuddleofdeath), [Sarah Schwartz](https://twitter.com/schwartzswartz), [Krystal](https://twitter.com/theekrystallee)
Traducteur: [CookingCryptos](https://twitter.com/CookingCryptos)
