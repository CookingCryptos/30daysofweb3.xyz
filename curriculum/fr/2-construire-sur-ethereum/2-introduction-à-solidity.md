---
title: Introduction à Solidity
description: Learn Solidity hands-on by writing, deploying, and testing your own simple smart contract in Remix.
optional: false
tweet: "Write a simple smart contract in Solidity on @EthereumRemix with #30DaysofWeb3 @womenbuildweb3 🔗"
---

![Introduction à Solidity](https://user-images.githubusercontent.com/15064710/180662387-02cf75b9-daf4-4a2d-ab07-0cf781453ce2.png)

## Écrire un Smart Contract

Le langage utilisé dans ce tutoriel est **Solidity** (_le langage utilisé pour construire des smart contracts Ethereum_) et ce même langage peut être utilisé pour déployer des smart contracts sur plusieurs chaînes telles que Binance Smart chain, Polygon ou Avalanche.

Dans cette section, vous apprendrez à développer des smart contracts avec le langage de programmation Solidity en travaillant sur un projet type. Vous apprendrez également à connecter les différents composants de Solidity (variables, types et fonctions) pour construire une DApp complète.

## ✋ Besoin d'aide ?

Si vous avez besoin d'aide, vérifiez si votre question a déjà été posée dans la **#section-2-help**. Si elle n'y figure pas, posez une question en indiquant tous les détails qui permettraient à un membre de l'équipe de vous aider facilement. Nous répondrons aux questions les plus fréquemment posées pendant les heures de bureau en direct, alors gardez un œil sur les #announcements pour celles-ci !

## Gm World

Nous commencerons par des extraits de code de base pour arriver à des projets plus avancés dans ce programme. Pour ce tutoriel, nous allons commencer par un projet pour débutants. Notre premier projet est une DApp pour stocker et récupérer des données de la blockchain.

```solidity
// SPDX-License-Identifier: GPL-3.0

pragma solidity >=0.7.0 <0.9.0;

contract Pets {
    string public myPet;

    function setPetName(string memory petName) public {
        myPet = petName;

    }
    function getPetName() public view returns(string memory){
        return myPet;
    }
}

```

La première ligne (avec le _SPDX-License-Identifier_) vous indique que le code source est sous licence GPL-3.0.

La deuxième ligne, "pragma solidity", indique la version de Solidity sur laquelle notre smart contract est écrit. Nous faisons cela afin de nous assurer d'utiliser le compilateur approprié.

Un contrat dans Solidity est similaire à une classe dans les langages de programmation orientés objet. Il s'agit d'une collection de code composée d'un constructor, de variables et de fonctions. Dans cet exemple, "Pets" est le nom du contrat. Dans notre contrat, nous avons une variable d'état de type string qui est publique et nous lui avons donné le nom de 'myPet'.

Nous avons défini deux fonctions, parfois appelées "getters" et "setters" en programmation. La première fonction, setPetName, définit l'état, ou la valeur, de la variable myPet. La seconde fonction, 'getPetName', récupère la valeur enregistrée dans la variable myPet. La grande majorité des fonctions que vous écrirez seront soit des setter, soit des getter.

### Anatomie d'une fonction

```solidity
function functionName(uint x, uint y) public view returns (uint){
    // function body here
}
```

Chaque fonction doit commencer par le mot-clé `function`, suivi de son nom `functionName`. Ce qui est placé entre les crochets (paramètres) sont les entrées, ou les valeurs que vous allez passer à votre fonction. `Public view returns` indique la visibilité de la fonction. Dans ce cas, elle définit qu'elle peut être accessible aux autres contrats, dénotée par le mot clé `public`. La fonction promet de ne pas modifier l'état de la blockchain, dénoté par le mot `view`. Enfin, `returns` définit que la fonction retournera une valeur, et définit également le type de données de cette sortie.

## Essayez vous-même

À l'aide de [Remix](https://remix.ethereum.org/#optimize=false&runs=200&evmVersion=null&version=soljson-v0.8.7+commit.e28d00a7.js), un IDE en ligne, créez un Smart Contract simple qui additionne deux nombres et renvoie la valeur obtenue.

Vous devez définir deux fonctions dans votre Smart Contract : une pour effectuer le calcul basé sur les deux nombres transmis par l'utilisateur, et une pour retourner la valeur de ce calcul. Nous allons écrire un getter pour récupérer la valeur actuelle de la variable et un setter pour additionner les deux nombres et mettre à jour la valeur de la variable.

Dans Remix, créez un nouveau fichier dans le dossier contracts, `add.sol`

![créer un fichier dans le dossier des contrats](https://user-images.githubusercontent.com/15346823/179375354-bac53920-028d-4463-8998-675d8a8f57b5.png)

Commencez par ajouter un identifiant de licence, suivi de la version du pragma :
pragma:

```solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity >=0.7.0 <0.9.0;
```

Ensuite, définissez votre contrat et à l'intérieur, définissez une variable de type uint (unsigned integer) et mettez-la à zéro.

```solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity >=0.7.0 <0.9.0;

contract AddNumbers{

  uint sum;

}
```

Ensuite, écrivez la fonction permettant d'additionner deux nombres transmis par l'utilisateur et une fonction permettant de renvoyer la valeur actuelle de la variable sum. Voici à quoi devrait ressembler votre contrat :

```solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity >=0.7.0 <0.9.0;

contract AddNums {
      uint sum;

   function addNums(uint x, uint y) public {
      sum = x + y;
}

   function getSum() public view returns (uint) {
     return sum;
}

}
```

Maintenant, nous allons compiler, déployer et tester notre contrat. Allez sur la 3ème icône en partant du haut et cliquez sur `Compile add.sol`.

![Compile contract](https://user-images.githubusercontent.com/20168921/182480413-347b90c0-8330-48cd-9e76-37298b5c9aea.png)

Déplacez-vous vers la 4ème icône en partant du haut et sélectionnez Remix VM dans le menu déroulant de la sélection de l'environnement (`environment`). Cela vous donnera un peu de faux éther pour pouvoir déployer et tester votre contrat.

> Remarque: [Remix a renommé cette option en Remix  VM](https://remix-ide.readthedocs.io/en/latest/run.html) - vous ne verrez plus la Javascript VM comme une option. Sélectionnez Remix VM !

![JavascriptVM](https://user-images.githubusercontent.com/15346823/179375210-bc843162-dcf0-4337-a9ed-2ca85a3fde7a.png)

Enfin, cliquez sur le bouton `Deploy` pour créer une instance de votre contrat avec laquelle nous allons interagir et tester que la fonction somme fonctionne comme prévu. Après quelques secondes, vous verrez un panneau `Deployed Contracts` en bas à gauche.

![Deploy contract](https://user-images.githubusercontent.com/20168921/182480524-81baf9d3-373a-45ee-9d1e-99f59fe1387e.png)

Passez deux integers, puis appuyez sur le bouton caret juste à côté de addNums et des deux integers que vous venez de passer. Un bouton 'transact' apparaît. Cliquez dessus et vous verrez un nouveau log indiquant la nouvelle transaction que vous venez de lancer.

![integers](https://user-images.githubusercontent.com/15346823/179375306-905213b2-2b60-4f9d-832d-3cb1a7dd1f43.png)


La fonction addNums ajoute les deux nombres, mais ne renvoie pas réellement la nouvelle valeur. Afin de vérifier que la fonction a fonctionné, nous devons appeler notre fonction getter. Cliquez sur le bouton `getSum`. Vous remarquerez qu'un nouveau log apparaît. Développez ce log en utilisant la flèche vers le bas et faites défiler jusqu'en bas pour trouver une valeur appelée `decoded output`.

Vous verrez que nous obtenons la bonne réponse - 8 ! Vous venez d'écrire votre premier Smart Contract :-)

![resultat](https://user-images.githubusercontent.com/15346823/179375323-dd99fa72-84a3-460f-bcf3-d7d1a977f94d.png)

🎉 Faites savoir à Twitter que vous venez d'écrire votre premier smart contract Solidity - cliquez sur le bouton Partager ci-dessous et collez le lien vers votre tweet dans **#builders-hype** pour que tout le monde puisse vous applaudir !

---

Écrivains: [Cami](https://twitter.com/camiinthisthang), [Fatma](https://twitter.com/fatima39_fatima),
Rédacteurs: [Busayo](https://twitter.com/AmoweO), [Sarah Schwartz](https://twitter.com/schwartzswartz), [Deborah Emeni](https://twitter.com/_emeni_deborah)
Traducteur: [CookingCryptos](https://twitter.com/CookingCryptos)
