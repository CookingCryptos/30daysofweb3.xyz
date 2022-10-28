---
title: Construire avec Lens
description: Create a simple web3-native social dapp using Lens Protocol.
tweet: "Create a simple web3-native social dapp using @LensProtocol with #30DaysofWeb3 @womenbuildweb3 🌱"
---

Pour ce tutoriel, vous allez travailler avec **Lens API**, une API GraphQL qui nous permet d'interagir rapidement avec le Lens social graph sans avoir à nous soucier de l'écriture de Solidity ou de l'indexation, de l'interrogation et de la validation des données. 

Ce tutoriel est basé sur la vidéo YouTube de [Nader Dabit's](https://twitter.com/dabit3), [Fullstack Web3 with Lens Protocol](https://www.youtube.com/watch?v=LcxOdWWL8xs&t=3s).

## Installation

Fork et clone notre repo de départ : https://github.com/womenbuildweb3/lens-api-starter

Vous pouvez trouver des instructions similaires pour forker et cloner un repo ici : https://www.30daysofweb3.xyz/en/curriculum/3-writing-your-smart-contract/1-dev-setup

Après avoir cloné le dépôt, ouvrez ce projet dans votre IDE et installez toutes les dépendances en exécutant cette commande dans votre terminal :

```bash
npm install
# or
yarn install
```

Ensuite, démarrez le serveur de développement :

```bash
npm run dev
# or
yarn dev
```

## Obtenir les profils recommandés

Dans le répertoire racine de votre projet, créez un fichier appelé `api.js`.

Copiez et collez la variable suivante.

```jsx
export const recommendedProfiles = `
  query RecommendedProfiles {
    recommendedProfiles {
      id
      name
      bio
      attributes {
        displayType
        traitType
        key
        value
      }
      followNftAddress
      metadata
      isDefault
      picture {
        ... on NftImage {
          contractAddress
          tokenId
          uri
          verified
        }
        ... on MediaSet {
          original {
            url
            mimeType
          }
        }
        __typename
      }
      handle
      coverPicture {
        ... on NftImage {
          contractAddress
          tokenId
          uri
          verified
        }
        ... on MediaSet {
          original {
            url
            mimeType
          }
        }
        __typename
      }
      ownedBy
      dispatcher {
        address
        canUseRelay
      }
      stats {
        totalFollowers
        totalFollowing
        totalPosts
        totalComments
        totalMirrors
        totalPublications
        totalCollects
      }
      followModule {
        ... on FeeFollowModuleSettings {
          type
          amount {
            asset {
              symbol
              name
              decimals
              address
            }
            value
          }
          recipient
        }
        ... on ProfileFollowModuleSettings {
        type
        }
        ... on RevertFollowModuleSettings {
        type
        }
      }
    }
  }
`;
```

Cette requête renvoie plusieurs profils populaires.

Configurons notre client urql pour pouvoir appeler notre requête. En haut du fichier, ajoutez :

```jsx
import { createClient } from 'urql';

const APIURL = "https://api-mumbai.lens.dev";

export const client = new createClient({
  url: APIURL,
});

```

Ensuite, depuis votre répertoire racine, trouvez le dossier pages et allez dans votre fichier `index.js`. Il devrait ressembler à ceci :

```jsx
import { useState, useEffect } from "react";
import Link from "next/link";
import Image from "next/image";
import { client, recommendedProfiles } from "../api";

export default function Home() {

  useEffect(() => {
    fetchProfiles();
  }, []);

  async function fetchProfiles() {
    try {
      const response = await client.query(recommendedProfiles).toPromise();
      console.log({ response });
    } catch (err) {
      console.log(err);
    }
  }

  return (
   <div></div>
  );
}

```

Dans votre terminal, exécutez `npm run dev` pour exécuter votre projet localement puis visitez http://localhost:3000 dans votre navigateur. Ouvrez la console de votre navigateur pour voir les données que nous avons récupérées de notre recommendedProfiles query. Cela devrait ressembler à quelque chose comme ceci -

![Screenshot of developer console showing a response object with data for lens profiles](https://i.imgur.com/CopLR6x.png)

Ne vous inquiétez pas si beaucoup de ces résultats ont des champs vides.

Maintenant que nous avons confirmé que notre requête fonctionne, nous allons afficher les profils. Mettez à jour votre composant Home pour qu'il ressemble à ceci -


```jsx
import { useState, useEffect } from "react";
import Link from "next/link";
import Image from "next/image";
import { client, recommendedProfiles } from "../api";
import Layout from "../components/Layout";
import Head from "next/head";
import HeroSection from "../components/HeroSection";

export default function Home() {
  const [profiles, setProfiles] = useState([]);

  useEffect(() => {
    fetchProfiles();
  }, []);

  async function fetchProfiles() {
    try {
      const response = await client.query(recommendedProfiles).toPromise();
      console.log({ response });
      setProfiles(response.data.recommendedProfiles);
    } catch (err) {
      console.log(err);
    }
  }

  return (
    <Layout>
      <Head>
        <title>Lensbook</title>
      </Head>
      <HeroSection />
      <div className="my-16 space-y-12 sm:grid sm:grid-cols-2 sm:gap-x-6 sm:gap-y-12 sm:space-y-0 md:grid-cols-3 md:gap-x-8 lg:grid-cols-4">
        {profiles.map((profile) => (
          <Link href={`/profile/${profile.id}`} key={profile.id}>
            <a className="flex flex-col items-center">
              {profile.picture &&
              profile.picture.original &&
              profile.picture.original.url.includes("lens.infura-ipfs.io") ? (
                <div className="relative w-60 h-60 bg-emerald-900 rounded">
                  <Image
                    src={profile.picture.original.url}
                    layout="fill"
                    objectFit="cover"
                    alt={profile.handle}
                    className="rounded"
                  />
                </div>
              ) : (
                <div className="bg-emerald-900 w-60 h-60 rounded" />
              )}
              <div className="mt-4 text-lg leading-6 font-medium text-center space-y-1">
                <h3>{profile.name}</h3>
                <p className="text-emerald-600">{profile.handle}</p>
              </div>
              <div className="text-gray-600 mt-2 grid grid-cols-2 gap-x-2 text-sm sm:text-base text-center">
                <p>
                  <span className="text-gray-900 font-medium">
                    {profile.stats.totalFollowers}
                  </span>{" "}
                  Followers
                </p>
                <p>
                  <span className="text-gray-900 font-medium">
                    {profile.stats.totalFollowing}
                  </span>{" "}
                  Following
                </p>
              </div>
            </a>
          </Link>
        ))}
      </div>
    </Layout>
  );
}
```

Votre page d'accueil devrait maintenant afficher une liste de profils d'objectifs populaires.

## Afficher un profil

Ensuite, nous pouvons créer une page qui affichera les détails de tout profil sur lequel nous cliquerons. Pour ce faire, nous pouvons utiliser le routage dynamique. Créez un nouveau dossier dans le dossier pages nommé `profile`. Dans ce dossier, créez un nouveau fichier appelé `[id].js` et ajoutez le code ci-dessous.

```jsx
import { useRouter } from "next/router";
import Layout from "../../components/Layout";
import Head from "next/head";
import Image from "next/image";

export default function Profile() {
  const router = useRouter();
  const { id } = router.query;
  return (
    <div>
      ID: {id}
    </div>
  );
}
```

Maintenant, si vous cliquez sur un profil sur votre page d'accueil, vous devriez voir l'identifiant de cet utilisateur sur la page du profil.

Nous voudrons montrer plus de détails sur l'utilisateur ici, donc nous aurons besoin de mettre en place une requête pour récupérer ces données. 

Dans notre fichier `api.js`, nous pouvons ajouter une nouvelle requête appelée getProfileById. Nous avons copié cette requête depuis la documentation de Lens ici : https://docs.lens.xyz/docs/get-profile

```jsx
export const getProfileById = `
  query Profile($id: ProfileId!) {
    profile(request: { profileId: $id }) {
      id
      name
      bio
      attributes {
        displayType
        traitType
        key
        value
      }
      followNftAddress
      metadata
      isDefault
      picture {
        ... on NftImage {
          contractAddress
          tokenId
          uri
          verified
        }
        ... on MediaSet {
          original {
            url
            mimeType
          }
        }
        __typename
      }
      handle
      coverPicture {
        ... on NftImage {
          contractAddress
          tokenId
          uri
          verified
        }
        ... on MediaSet {
          original {
            url
            mimeType
          }
        }
        __typename
      }
      ownedBy
      dispatcher {
        address
        canUseRelay
      }
      stats {
        totalFollowers
        totalFollowing
        totalPosts
        totalComments
        totalMirrors
        totalPublications
        totalCollects
      }
      followModule {
        ... on FeeFollowModuleSettings {
          type
          amount {
            asset {
              symbol
              name
              decimals
              address
            }
            value
          }
          recipient
        }
        ... on ProfileFollowModuleSettings {
          type
        }
        ... on RevertFollowModuleSettings {
          type
        }
      }
    }
  }
`;
```

De retour dans notre fichier `/pages/profile/[id].js`, nous pouvons importer notre client et notre requête en haut du fichier, ainsi que `useState` et `useEffect`.

```jsx
import { useState, useEffect } from "react";
import { client, getProfileById } from "../../api";
```

A l'intérieur de votre fonction `Profile`, créez une fonction async appelée `fetchProfile`, où nous pouvons récupérer nos données. Nous pouvons utiliser `useEffect` pour exécuter cette fonction si la variable `id` n'est pas nulle. Assurez-vous d'ajouter le `id` au tableau des dépendances.

```jsx
useEffect(() => {
  if (id) {
    fetchProfile();
  }
}, [id]);
 
async function fetchProfile() {
      
}
```

Dans la fonction fetchProfile, nous pouvons utiliser une instruction try...catch pour détecter toute erreur lors de la récupération des données de profil.

```jsx
async function fetchProfile(){
  try {
    const response = await client.query(getProfileById, { id }).toPromise();
    console.log("PROFILE:", response);
  } catch(error) {
    console.log("ERROR:", error);
  }
}
```

Vous devriez maintenant être en mesure de voir les données du profil enregistrées dans la console. 

Ensuite, au lieu de simplement enregistrer ces données, vous pouvez les sauvegarder dans l'état. En haut de la fonction `Profile`, utilisez le hook `useState` pour déclarer les variables `profile` et `setProfile` :

```jsx
const [profile, setProfile] = useState()
```

Maintenant vous pouvez utiliser `setProfile` pour stocker les données de profil de votre réponse. Pour le tester, vous pouvez essayer de rendre le handle du profil si la variable profile n'est pas nulle.

```jsx
async function fetchProfile(){
  try {
    const response = await client.query(getProfileById, { id }).toPromise();
    console.log("PROFILE:", response);
    setProfile(response.data.profile);
  } catch(error) {
    console.log("ERROR:", error);
  }
}

return (
    <div>
        {profile && <div>{profile.handle}</div>}
    </div>
)
```

Ensuite, vous pouvez mettre à jour l'interface utilisateur de notre page de profil pour afficher plus d'informations sur le profil. Nous avons ajouté un peu de style ici pour vous aussi afin de rendre la page plus agréable à regarder.

```jsx
return (
  <Layout>
    <Head>
      <title>{profile ? profile.handle : "Lensbook"}</title>
    </Head>
    <div className="my-12">
      {profile && (
        <div className="flex flex-wrap md:flex-nowrap items-start w-full">
          <div className="w-full md:w-auto mb-4 md:mr-8">
            {profile.picture &&
            profile.picture.original &&
            profile.picture.original.url.includes("lens.infura-ipfs.io") ? (
              <div className="relative w-60 h-60 bg-emerald-900 rounded mx-auto">
                <Image
                  src={profile.picture.original.url}
                  layout="fill"
                  objectFit="cover"
                  alt={profile.handle}
                  className="rounded"
                />
              </div>
            ) : (
              <div className="bg-emerald-900 w-60 h-60 rounded mx-auto" />
            )}
          </div>
          <div className="grow-1 w-full">
            <div className="text-center md:text-left">
              <h1 className="text-3xl font-bold text-gray-900 sm:text-4xl sm:tracking-tight mb-1">
                {profile.name}
              </h1>
              <h2 className="text-xl font-bold text-emerald-500 sm:text-2xl sm:tracking-tight mb-2">
                {profile.handle}
              </h2>
              <div className="flex flex-wrap gap-x-2 text-gray-600 text-sm sm:text-base mb-4 justify-center md:justify-start">
                <p>
                  <span className="text-gray-900 font-medium">
                    {profile.stats.totalFollowers}
                  </span>{" "}
                  Followers
                </p>
                <p>
                  <span className="text-gray-900 font-medium">
                    {profile.stats.totalFollowing}
                  </span>{" "}
                  Following
                </p>
              </div>
              <p className="mb-4">{profile.bio}</p>
              {/* Add connect and follow buttons here */}
            </div>
            {/* Add publications here */}
          </div>
        </div>
      )}
    </div>
  </Layout>
)
```

Ensuite, nous voulons être en mesure d'afficher toutes les publications que cet utilisateur a créées. D'après la documentation de Lens, "les publications sont de trois types principaux : les posts, les comments et les mirrors. Les posts sont l'objet de base, les mirrors et les comments offrant des fonctionnalités supplémentaires." Si vous êtes familier avec Twitter, considérez les posts comme des tweets et les mirrors comme des retweets. Les publications seraient comme la section Tweets & Replies du profil Twitter d'une personne. 

De retour dans notre fichier `api.js`, nous pouvons ajouter cette nouvelle requête pour récupérer les publications d'un certain utilisateur. Ici, nous la nommons `getPublicationsById`. Note : nous avons copié cette requête depuis la documentation de lens ici : https://docs.lens.xyz/docs/get-publications. C'est une requête assez large, et vous ne voudrez probablement pas utiliser toutes ces données. Nous vous recommandons de supprimer tous les champs que vous n'utiliserez pas.

```jsx
export const getPublicationsById = `
  query Publications ($id: ProfileId!) {
    publications(request: {
      profileId: $id,
      publicationTypes: [POST, COMMENT, MIRROR],
      limit: 10
    }) {
      items {
        __typename
        ... on Post {
          ...PostFields
        }
        ... on Comment {
          ...CommentFields
        }
        ... on Mirror {
          ...MirrorFields
        }
      }
      pageInfo {
        prev
        next
        totalCount
      }
    }
  }

  fragment MediaFields on Media {
    url
    mimeType
  }

  fragment ProfileFields on Profile {
    id
    name
    bio
    attributes {
      displayType
      traitType
      key
      value
    }
    isFollowedByMe
    isFollowing(who: null)
    followNftAddress
    metadata
    isDefault
    handle
    picture {
      ... on NftImage {
        contractAddress
        tokenId
        uri
        verified
      }
      ... on MediaSet {
        original {
          ...MediaFields
        }
      }
    }
    coverPicture {
      ... on NftImage {
        contractAddress
        tokenId
        uri
        verified
      }
      ... on MediaSet {
        original {
          ...MediaFields
        }
      }
    }
    ownedBy
    dispatcher {
      address
    }
    stats {
      totalFollowers
      totalFollowing
      totalPosts
      totalComments
      totalMirrors
      totalPublications
      totalCollects
    }
    followModule {
      ... on FeeFollowModuleSettings {
        type
        amount {
          asset {
            name
            symbol
            decimals
            address
          }
          value
        }
        recipient
      }
      ... on ProfileFollowModuleSettings {
      type
      }
      ... on RevertFollowModuleSettings {
      type
      }
    }
  }

  fragment PublicationStatsFields on PublicationStats {
    totalAmountOfMirrors
    totalAmountOfCollects
    totalAmountOfComments
  }

  fragment MetadataOutputFields on MetadataOutput {
    name
    description
    content
    media {
      original {
        ...MediaFields
      }
    }
    attributes {
      displayType
      traitType
      value
    }
  }

  fragment Erc20Fields on Erc20 {
    name
    symbol
    decimals
    address
  }

  fragment CollectModuleFields on CollectModule {
    __typename
    ... on FreeCollectModuleSettings {
        type
        followerOnly
        contractAddress
    }
    ... on FeeCollectModuleSettings {
      type
      amount {
        asset {
          ...Erc20Fields
        }
        value
      }
      recipient
      referralFee
    }
    ... on LimitedFeeCollectModuleSettings {
      type
      collectLimit
      amount {
        asset {
          ...Erc20Fields
        }
        value
      }
      recipient
      referralFee
    }
    ... on LimitedTimedFeeCollectModuleSettings {
      type
      collectLimit
      amount {
        asset {
          ...Erc20Fields
        }
        value
      }
      recipient
      referralFee
      endTimestamp
    }
    ... on RevertCollectModuleSettings {
      type
    }
    ... on TimedFeeCollectModuleSettings {
      type
      amount {
        asset {
          ...Erc20Fields
        }
        value
      }
      recipient
      referralFee
      endTimestamp
    }
  }

  fragment PostFields on Post {
    id
    profile {
      ...ProfileFields
    }
    stats {
      ...PublicationStatsFields
    }
    metadata {
      ...MetadataOutputFields
    }
    createdAt
    collectModule {
      ...CollectModuleFields
    }
    referenceModule {
      ... on FollowOnlyReferenceModuleSettings {
        type
      }
    }
    appId
    hidden
    reaction(request: null)
    mirrors(by: null)
    hasCollectedByMe
  }

  fragment MirrorBaseFields on Mirror {
    id
    profile {
      ...ProfileFields
    }
    stats {
      ...PublicationStatsFields
    }
    metadata {
      ...MetadataOutputFields
    }
    createdAt
    collectModule {
      ...CollectModuleFields
    }
    referenceModule {
      ... on FollowOnlyReferenceModuleSettings {
        type
      }
    }
    appId
    hidden
    reaction(request: null)
    hasCollectedByMe
  }

  fragment MirrorFields on Mirror {
    ...MirrorBaseFields
    mirrorOf {
    ... on Post {
        ...PostFields         
    }
    ... on Comment {
        ...CommentFields         
    }
    }
  }

  fragment CommentBaseFields on Comment {
    id
    profile {
      ...ProfileFields
    }
    stats {
      ...PublicationStatsFields
    }
    metadata {
      ...MetadataOutputFields
    }
    createdAt
    collectModule {
      ...CollectModuleFields
    }
    referenceModule {
      ... on FollowOnlyReferenceModuleSettings {
        type
      }
    }
    appId
    hidden
    reaction(request: null)
    mirrors(by: null)
    hasCollectedByMe
  }

  fragment CommentFields on Comment {
    ...CommentBaseFields
    mainPost {
      ... on Post {
        ...PostFields
      }
      ... on Mirror {
        ...MirrorBaseFields
        mirrorOf {
          ... on Post {
            ...PostFields         
          }
          ... on Comment {
            ...CommentMirrorOfFields       
          }
        }
      }
    }
  }

  fragment CommentMirrorOfFields on Comment {
    ...CommentBaseFields
    mainPost {
      ... on Post {
        ...PostFields
      }
      ... on Mirror {
        ...MirrorBaseFields
      }
    }
  }
`;
```

Maintenant dans `pages/profile/[id].js` vous pouvez mettre à jour l'import en haut pour inclure cette fonction.

```jsx
import { client, getProfileById, getPublicationsById } from "../../api"
```

You will also add a new variable to keep track of our publications. To keep it short, we’ll call it pubs and set it to an empty array with `useState`.

```jsx
const [pubs, setPubs] = useState([])
```

Vous allez également ajouter une nouvelle variable pour garder la trace de nos publications. Pour faire court, nous l'appellerons pubs et la mettrons dans un tableau vide avec `useState`.

```jsx
async function fetchProfile() {
  try {
    const response = await client.query(getProfileById, { id }).toPromise();
    console.log("PROFILE:", response);
    setProfile(response.data.profile);

    const publications = await client.query(getPublicationsById, { id }).toPromise();
    console.log("PUBS!", publications);
    setPubs(publications.data.publications.items);
  } catch (error) {
    console.log("ERROR:", error);
  }
}
```

Ensuite, vous pouvez cartographier les publications pour afficher une liste de chacune d'entre elles :

```jsx
{/* Add publications here */}
{pubs.length > 0 && (
  <div className="border-t-2 border-gray-100 my-8 py-8 flex flex-col space-y-8">
    {pubs.map((p, index) => (
      <div key={p.id}>
        <p className="font-bold">{p.__typename}</p>
        <p>{p.metadata.content}</p>
        <p>{p.metadata.name}</p>
      </div>
    ))}
  </div>
)}

```

## Configurer votre wallet

Avant de continuer, nous voulons nous assurer que nous avons un wallet qui peut se connecter au testnet de Polygon et qui a du Matic, afin que nous puissions interagir avec les smart contracts de Lens.

Si vous n'avez pas encore configuré votre wallet, consultez notre tutoriel ici : https://www.30daysofweb3.xyz/en/curriculum/1-getting-started/5-set-up-your-wallet.

Vous aurez également besoin d'avoir des MATIC de test dans votre wallet. Vous pouvez demander des MATIC de test à partir du faucet de Polygon Mumbai ici : https://faucet.polygon.technology/.

## ✋ Need Help?

If you need help, check to see if your question has already been asked in **#section-9-help**. If you don't see it in there, post a question with any details that would make it easy for a team member to help you. 

## Suivre l'utilisateur

Nous voulons ajouter un bouton "suivre" pour que les utilisateurs puissent suivre un profil qu'ils aiment. Pour suivre un profil avec Lens Protocol, un utilisateur devra interagir avec le contrat Lens.

Vous pouvez trouver toutes les adresses des contrats déployés de Lens Protocol ici :
https://docs.lens.xyz/docs/deployed-contract-addresses.

Pour ce tutoriel, nous allons interagir avec les contrats LensHub qui sont déployés sur Mumbai Testnet.

Dans le fichier `pages/profile/[id].js`, ajoutez une variable appelée `CONTRACT_ADDRESS` et donnez-lui l'adresse du contrat Lens.

```jsx
const CONTRACT_ADDRESS = "0x60Ae865ee4C725cd04353b5AAb364553f56ceF82"
```

Ensuite, dans le répertoire racine de votre projet, créez un fichier appelé `abi.json`.

Ouvrez ce [lien] (https://mumbai.polygonscan.com/address/0x8C1f82e8AAD9399f52DcF224b77f33d5c1719241#code), et faites défiler la page jusqu'à ce qu'il soit indiqué "Contract ABI".

![Contract ABI](https://i.imgur.com/AbxpwRh.png)

Copiez et collez l'ABI dans `abi.json`. 

Dans `pages/profile/[id].js`, importez l'ABI.

```jsx
import ABI from "../../abi.json";
```

Avant de pouvoir ajouter le bouton de suivi, nous devons permettre à l'utilisateur de connecter son portefeuille.

Tout d'abord, créez une nouvelle variable d'état pour référencer le wallet connecté.

```jsx
const [accounts, setAccounts] = useState(null);
```

Ensuite, ajoutez cette fonction connectWallet après `fetchProfile`.

```jsx
async function connectWallet() {
  const accounts = await window.ethereum.request({
    method: "eth_requestAccounts",
  });
  console.log("accounts: ", accounts);
  setAccounts(accounts);
}

```

Ensuite, dans votre interface utilisateur, ajoutez un bouton Connect Wallet qui appelle la fonction `connectWallet` au clic.

```jsx
{/* Add connect and follow buttons here */}
                
<button
  onClick={connectWallet}
  type="button"
  className="inline-flex items-center px-4 py-2 border border-transparent text-base font-medium rounded-md text-emerald-700 bg-emerald-100 hover:bg-emerald-200 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-emerald-500"
>
  Connect Wallet
</button>
                
```

Enregistrez votre fichier puis confirmez que vous pouvez connecter votre wallet en cliquant sur le bouton Connect Wallet et en voyant le tableau des comptes dans votre console.

Maintenant que l'utilisateur peut connecter son wallet, nous pouvons supporter la fonctionnalité suivante.

Importez ethers en haut du fichier.

```jsx
import { ethers } from 'ethers'
```

Si vous vous souvenez de ce qui a été dit plus tôt dans le programme, ethers.js est une bibliothèque JavaScript qui permet aux développeurs d'interagir avec Ethereum.

Ensuite, ajoutez cette fonction `followUser` après `connectWallet`.

```jsx
async function followUser() {
  const provider = new ethers.providers.Web3Provider(window.ethereum);
  const signer = provider.getSigner();

  const contract = new ethers.Contract(CONTRACT_ADDRESS, ABI, signer);

  try {
    const tx = await contract.follow([id], [0x0]);
    await tx.wait();
    console.log("Followed user successfully");
  } catch (err) {
    console.log("Failed to follow user due to", err);
  }
}
```

Parcourons cette fonction ligne par ligne.

Tout d'abord, nous créons un fournisseur qui nous permet de nous connecter au réseau Ethereum par le biais du wallet de l'utilisateur.

```jsx
const provider = new ethers.providers.Web3Provider(window.ethereum);
```

Ensuite, nous obtenons du fournisseur le signataire qui sera utilisé pour signer et envoyer les transactions.

```jsx
const signer = provider.getSigner();
```

Puis nous créons un objet contrat pour pouvoir interagir avec le smart contract de LensHub.

```jsx
const contract = new ethers.Contract(CONTRACT_ADDRESS, ABI, signer);
```

Ensuite, nous créons un objet de contrat afin de pouvoir interagir avec le smart contract de LensHubNext, nous voulons appeler une fonction qui nous permettra de suivre les utilisateurs. Nous pouvons naviguer dans les fonctions définies dans le Smart Contract de LensHub à https://docs.lens.xyz/docs/functions.

Passons à la fonction follow() à https://docs.lens.xyz/docs/functions#follow.

La fonction follow() prend profileIds comme paramètre. profileIds est un tableau d'IDs du profil que vous voulez suivre. Étant donné un tableau d'identifiants de profils, la fonction follow() extrait un NFT de suivi pour chaque identifiant de profil.

Pour revenir à la fonction followUser, notez que nous appelons la fonction follow() du contrat et que nous lui passons l'identifiant de la page de profil sur laquelle nous nous trouvons.

```jsx
const tx = await contract.follow([id], [0x0]);
```

Ensuite, nous attendons que la transaction soit terminée.

```jsx
await tx.wait();
```

Nous enveloppons cela dans un bloc `try..catch` au cas où la transaction échoue.

Maintenant que nous comprenons comment fonctionne followUser, ajoutez un bouton Follow qui appelle followUser au clic sous les informations du profil. Mettez à jour votre code comme suit

```jsx
{/* Add connect and follow buttons here */}
{accounts ? (
  <button
    onClick={followUser}
    type="button"
    className="inline-flex items-center px-4 py-2 border border-transparent text-base font-medium rounded-md shadow-sm text-white bg-emerald-600 hover:bg-emerald-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-emerald-500"
  >
    Follow {profile.handle}
  </button>
) : (
  <button
    onClick={connectWallet}
    type="button"
    className="inline-flex items-center px-4 py-2 border border-transparent text-base font-medium rounded-md text-emerald-700 bg-emerald-100 hover:bg-emerald-200 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-emerald-500"
  >
    Connect Wallet
  </button>
)}

```

Ici, nous vérifions si nous avons connecté notre wallet. Si le wallet est connecté, nous affichons le bouton Follow.

Enregistrez votre fichier mis à jour, connectez votre wallet, puis cliquez sur Follow. Confirmez que vous suivez bien ce profil en vérifiant votre console.

## Conclusion

Les médias sociaux ne sont qu'une application du graphe social, et maintenant avec Lens, vous avez les outils pour créer des applications qui reflètent les relations significatives que nous avons on-chain.

Pour être éligible à l'obtention d'un identifiant Lens et à la candidature à l'accélérateur, assurez-vous de soumettre vos projets web3rsvp et Lens dans le formulaire de soumission de projet qui se trouve à la [fin du programme](https://www.30daysofweb3.xyz/en/curriculum/9-wrapping-up/3-finale). Si vous avez déjà soumis votre projet web3rsvp, consultez à nouveau le formulaire et mettez à jour votre réponse.

## 👀 Alpha

L'accélérateur BUIDL arrive à grands pas. Plus de détails seront présentés officiellement dans les prochains jours, mais quelques alpha :

COMMENCEZ À CONSTRUIRE QUELQUE CHOSE. En plus de nos subventions sans engagement, nous attribuerons également **20K$** de subventions aux équipes construisant avec Lens. Alors sautez dans le serveur, trouvez une équipe, et commencez à réfléchir !

---

Adapté de [Nader Dabit's Lens API tutorial](https://www.youtube.com/watch?v=LcxOdWWL8xs&ab_channel=NaderDabit)

Écrivains: [Sarah Schwartz](https://twitter.com/schwartzswartz), [Sarah Z](https://twitter.com/haegeez),
Rédacteurs: [Cami](https://twitter.com/camiinthisthang)
Traducteur: [CookingCryptos](https://twitter.com/CookingCryptos)
