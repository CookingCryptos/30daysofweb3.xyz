---
title: Héberger votre code avec Radicle
description: Learn how to host your code on Radicle, a decentralized code collaboration network.
optional: true
optionalMsg: If you've dabbled with Radicle or want to bookmark for this later, feel free to jump ahead!
optionalNextPath: /en/curriculum/9-wrapping-up/3-finale
tweet: "Host code for a full-stack dapp on @radicle with #30DaysofWeb3 @womenbuildweb3 💻"
---

Maintenant que nous avons terminé notre application, nous pouvons télécharger notre code sur Radicle pour le garder en sécurité. **Radicle** est un _réseau de pair à pair pour le stockage de dépôts git conçu pour être exempt de censure_. Vous pouvez utiliser Radicle gratuitement de la même manière que vous utiliseriez GitHub ou tout autre site d'hébergement de dépôts git.

L'avantage majeur d'utiliser Radicle est qu'il s'agit d'un protocole décentralisé plutôt que d'une plateforme centralisée. Cela signifie qu'il ne peut y avoir de point de défaillance unique entraînant la perte ou la censure de votre contenu.

Cette section est facultative, alors ne vous inquiétez pas si vous avez des problèmes. Si vous êtes vraiment enthousiaste à propos de Radicle et que vous avez besoin d'aide avec cette section, contactez-nous via discord !

## Installation de Radicle CLI

Vous pouvez trouver la documentation officielle montrant comment installer le Radicle CLI ci-dessous :

https://docs.radicle.xyz/getting-started
https://github.com/radicle-dev/radicle-cli

### Installation du CLI pour Mac

Avant d'installer le CLI Radicle, nous devons installer quelques dépendances. Nous allons d'abord installer Rust et Cargo en exécutant les commandes ci-dessous dans l'ordre :

```
curl https://sh.rustup.rs -sSf | sh
```

```
source $HOME/.cargo/env
```

Ensuite, nous allons télécharger cmake ici : https://cmake.org/download/.

Installez l'application et déplacez-la dans votre dossier Applications, ouvrez-la et sélectionnez Outils → Comment installer pour une utilisation en ligne de commande dans la barre d'outils.

! [Capture d'écran de How to Install for Command Line Use dans la barre d'outils](https://i.imgur.com/GDLGFv7.png)

Choisissez l'une des options dans la pop-up pour installer cmake pour la ligne de commande. Si vous n'êtes pas sûr de l'option à utiliser, utilisez la commande ci-dessous :

```
sudo "/Applications/CMake.app/Contents/bin/cmake-gui" --install
```

Vous pouvez maintenant exécuter la commande ci-dessous pour installer définitivement la CLI de Radicle. Cela peut prendre quelques minutes.

```
cargo install --force --locked --git https://seed.alt-clients.radicle.xyz/radicle-cli.git radicle-cli
```

Exécutez `rad` pour tester si l'installation a réussi. Vous devriez voir l'information ci-dessous :

![Common `rad` commands](https://i.imgur.com/A9wZqqq.png)

## Créer un Repo Radicle

Pour créer un nouveau repo, ouvrez votre dossier projet dans votre terminal, et exécutez `rad auth` pour créer notre compte utilisateur. Entrez un nom d'utilisateur et un mot de passe, et le CLI va générer votre Peer ID Radicle (device id) et votre URN personnel (user id). Vous pouvez toujours obtenir ces informations plus tard en exécutant `rad self` dans votre terminal.

**Note:** _Il n'y a actuellement aucun moyen de récupérer une phrase de passe perdue ou oubliée, alors stockez-la en toute sécurité !_

Ensuite, vous pouvez exécuter `rad init`, et entrer un nom et une description pour le repo. Cela devrait générer un ID de projet. Vous pouvez toujours récupérer cet identifiant en exécutant `rad .` .

Vous pouvez maintenant pousser le repo vers Radicle en exécutant `rad push`. La première fois que vous poussez votre code, il vous sera demandé de sélectionner un noeud. Vous pouvez sélectionner n'importe quelle option.

Maintenant votre code est hébergé sur Radicle ! 🎉 Vous devriez voir où vous pouvez voir votre code hébergé.

Vous n'avez pas besoin de vous soucier du stockage de celui-ci, puisque vous pouvez toujours exécuter `rad ls` pour voir une liste de tous les dépôts que vous avez poussés sur Radicle.

Si vous voyez une erreur dans votre navigateur qui mentionne votre réseau, connectez-vous à votre Coinbase Wallet ou Metamask et changez le réseau en Ethereum Mainnet.

### Faire des changements

Vous pouvez ajouter des changements avec `git add` et `git commit` comme vous le feriez avec n'importe quel dépôt git. Il suffit de lancer `rad push` pour pousser vos commits vers Radicle.

### Clonage

Si vous voulez partager votre code avec d'autres personnes qui ont également le CLI de Radicle installé, ils peuvent exécuter `rad clone` avec l'URN du projet et le noeud d'origine à cloner.

```
rad clone rad:git:hnrkknc6ntqasrnej6ous5krdw464etyo3i7y --seed pine.radicle.garden
```

---

Écrivains: [Sarah Schwartz](https://twitter.com/schwartzswartz)
Traducteur: [CookingCryptos](https://twitter.com/CookingCryptos)
