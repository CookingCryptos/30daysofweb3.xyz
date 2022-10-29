---
title: Introduction a la Blockchain
description: Learn about what blockchain technology is and how transactions are executed on the blockchain.
optional: true
optionalMsg: Si vous êtes familier avec les principes fondamentaux du web3 et que vous avez déjà votre propre wallet de crypto-monnaies, n'hésitez pas à passer à la section suivante !
optionalNextPath: /fr/curriculum/2-construire-sur-ethereum/0-architecture-client-serveur
tweet: "Learn about blockchains and transactions with #30DaysofWeb3 @womenbuildweb3 🌐"
---

![Intro to Blockchain](https://user-images.githubusercontent.com/15064710/180661895-aa374dae-299a-46f0-a3d6-de8f7796936c.png)

## Qu'est ce que la Blockchain?

La **Blockchain** est un _système en expansion qui enregistre des informations d'une manière qui rend difficile ou presque impossible le piratage du système_. Les informations sont enregistrées dans des **blocks**, qui sont comme des _petites listes d'enregistrements_.

Il existe actuellement au moins [1000 blockchains avec au moins quatre types de réseaux de blockchains](https://earthweb.com/how-many-blockchains-are-there/#:~:text=Currently%2C%20there%20least,platforms%20provided%20in%20this%20industry.). Chaque bloc contient un **hash** (une longue chaîne de caractères représentant une donnée spécifique) du bloc précédent ainsi que d'autres informations utiles telles qu'un horodatage et des données de transaction. Ce processus forme une chaîne de données, autrement appelée **Blockchain**.

La technologie Blockchain est très polyvalente et peut être utilisée de diverses manières. Elle est utilisée pour sécuriser les informations personnelles, les redevances des artistes, les tokens non fongibles, les biens immobiliers, et [plus](https://www.fool.com/investing/stock-market/market-sectors/financials/blockchain-stocks/blockchain-applications/). Elle peut également faciliter les processus de vote, en garantissant que personne ne peut voter plus d'une fois ou altérer les votes de quelque manière que ce soit.

## Comment fonctionnent les transactions sur la Blockchain ?

La première étape d'une transaction sur la blockchain commence généralement par une demande de transaction de la part d'un utilisateur. Toute opération d'"écriture" sur la blockchain est une transaction. Il peut s'agir de déployer un contrat intelligent sur la chaîne, d'acheter un NFT, d'acheter un nom ENS, etc.

Les transactions sont des demandes pour valider votre action et l'ajouter à la chaîne. Afin d'exécuter avec succès une transaction sur la blockchain, un **frais de gas** (_un frais de transaction sur la blockchain_) est généralement requis.

Lorsqu'il y a beaucoup de trafic et une forte demande pour le réseau, les frais de gas augmentent parce que l'espace de bloc est limité, et donc les mineurs peuvent demander des frais plus élevés afin de donner la priorité aux transactions qu'ils veulent traiter. Pensez-y comme au Surge Pricing d'Uber : si vous êtes dans un aéroport avec un tas de gens qui prennent des Ubers, les prix augmentent et les temps d'attente augmentent.

**Les frais de gas** sont _quelque chose que tous les utilisateurs doivent payer afin d'exécuter une fonction sur la blockchain_. La quantité de gas à payer varie en fonction de la blockchain que vous utilisez, ainsi que d'autres facteurs comme un trafic élevé. Selon [Gemini](https://www.gemini.com/cryptopedia/what-are-gas-fees-gwei-gas-fees-eth-ether-transaction-fee), le montant de ces frais de transaction peut varier de moins de 0,0001 USD à plus de 100 USD.

Une fois que la transaction a été demandée, elle est authentifiée et ajoutée à un **bloc** (_qui représente un ensemble de transactions sur la blockchain_). Ces blocs ont chacun une capacité de stockage maximale, de sorte qu'une fois cette capacité atteinte, les blocs sont verrouillés et liés au bloc précédemment verrouillé. En outre, ces blocs contiennent des informations telles que des signatures numériques, un horodatage et toute autre information importante. Le bloc est envoyé à l'ensemble des **nœuds** du réseau (_participants à la blockchain_).

Ensuite, les nœuds valident la transaction et reçoivent une récompense (généralement, la récompense est la principale crypto-monnaie de la blockchain) pour avoir participé au processus de validation. Puis, le bloc est officiellement ajouté à la blockchain existante. Après cela, la blockchain reçoit une mise à jour à travers tout le réseau et reflète officiellement cette transaction. La transaction est maintenant terminée. Si vous souhaitez en savoir plus sur le fonctionnement des transactions, nous vous recommandons de consulter cet article utile de [Euromoney Learning](https://www.euromoney.com/learning/blockchain-explained/how-transactions-get-into-the-blockchain).

Maintenant que nous en savons un peu plus sur le fonctionnement des transactions de la blockchain, nous allons nous pencher sur les contrats intelligents ou smart contracts !

---

Écrivains: [Kristen](https://twitter.com/CuddleofDeath),
Rédacteurs: [Deborah Emeni](https://twitter.com/_emeni_deborah)
Traducteur: [CookingCryptos](https://twitter.com/CookingCryptos)


