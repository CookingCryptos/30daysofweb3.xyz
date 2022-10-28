---
title: Construire sur Ethereum
description: Learn about client server architecture in web2 and open, composable backends in web3.
optional: false
tweet: "Understand composability in web3 with #30DaysofWeb3 @womenbuildweb3 🔗"
---

## Architecture client-serveur


Pour bien comprendre la différence fondamentale entre le web2 et le web3, il faut comprendre l'architecture client-serveur traditionnelle pour comprendre le concept de backends ouverts et composables.

Une application client est celle avec laquelle l'utilisateur interagit réellement, celle qui affiche le contenu. Une application serveur est celle qui envoie le contenu, ou la ressource, à votre application client. Une application serveur est un programme qui s'exécute quelque part, qui écoute et qui attend une demande.

La raison principale de cette séparation est de sécuriser les informations sensibles. L'ensemble de votre application client est téléchargé dans le navigateur, et toutes les données sont accessibles à toute personne accédant à votre page Web.

Cette architecture permet de protéger des éléments tels que vos clés API, vos données personnelles, etc. Aujourd'hui, des outils modernes comme Next.js et Netlify permettent aux développeurs d'exécuter du code serveur dans la même application que leur application client, sans avoir besoin d'une application serveur dédiée.

### Pourquoi utilisons-nous une architecture client-serveur ?

Imaginons que vous construisiez une application Web de météo, par exemple. L'application météo avec laquelle votre utilisateur va interagir est l'application cliente - elle possède des boutons, une barre de recherche et affiche des données comme le nom de la ville, la température actuelle, l'indice de qualité de l'air, etc.

Cette application météo ne doit pas coder directement chaque ville et ses informations météorologiques. Cela rendrait l'application lourde et lente, prendrait une éternité à rechercher et à ajouter manuellement à une base de données. Ce serait un casse-tête à mettre à jour chaque jour.

Au lieu de cela, l'application peut accéder aux données météorologiques par ville en utilisant l'API Web de la météo. Votre application recueille l'emplacement de l'utilisateur, puis envoie une requête au serveur en disant : "Hé, envoyez-moi les informations météorologiques pour cette ville spécifique."

En fonction de ce que vous essayez d'obtenir, vous utiliserez les différentes méthodes de requête disponibles. Le serveur renvoie une réponse contenant les informations météorologiques et quelques autres éléments, selon la manière dont l'API est écrite. Il peut également renvoyer des éléments tels qu'un horodatage, la région dans laquelle se trouve la ville, etc.

Votre application client communique avec une application serveur exécutée quelque part, dont la seule tâche consiste à écouter en permanence les demandes adressées à cette adresse. Lorsqu'elle reçoit une demande, elle s'efforce d'y répondre, soit en lisant une base de données, une autre API, un fichier local ou un calcul programmatique basé sur les données que vous lui transmettez.

### Comment cela se traduit-il dans le développement de la blockchain ?

Au lieu d'avoir une application serveur, les développeurs de blockchains ont eu cette idée de backends ouverts et composables, appelés smart contracts. Vous créerez un smart contract qui gère la logique de la création d'un nouvel événement, de l'inscription à un nouvel événement, etc. Votre application client sera chargée d'agréger et d'afficher tous les événements qui ont été créés, de montrer un tableau de bord à votre utilisateur indiquant les événements passés et à venir, etc.

Lisez l'article complet sur l'architecture client-serveur, les API et les méthodes de requête HTTP dans [cet article](https://www.freecodecamp.org/news/http-request-methods-explained/).

---

Écrivains: [Cami](https://twitter.com/camiinthisthang),
Rédacteurs: [Deborah Emeni](https://twitter.com/_emeni_deborah), [Christina Mills](https://twitter.com/bombayonchain)
Traducteur: [CookingCryptos](https://twitter.com/CookingCryptos)

