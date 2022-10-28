---
title: Charger vos données d'événement 
description: Learn how to upload data to decentralized storage network using Web3.Storage.
optional: false
tweet: "Use @Web3Storage in a full-stack dapp with #30DaysofWeb3 @womenbuildweb3 🗂"
---

Créez un nouveau dossier dans le dossier `pages` appelé `api`, et créez un nouveau fichier dans ce dossier appelé `store-event-data.js`.

En haut du fichier, nous devrons importer quelques aides de `web3.storage` et du module `path`.

```javascript
import { Web3Storage, File, getFilesFromPath } from "web3.storage";
const { resolve } = require("path");
```

Nous devrons exporter une fonction de gestion par défaut pour gérer les requêtes entrantes. Ici, nous pouvons vérifier si la requête est une requête `POST`, et retourner une erreur si ce n'est pas le cas. Sinon, nous pouvons stocker les données de l'événement.

```javascript
export default async function handler(req, res) {
  if (req.method === "POST") {
    return await storeEventData(req, res);
  } else {
    return res
      .status(405)
      .json({ message: "Method not allowed", success: false });
  }
}
```

Ensuite, nous allons créer une nouvelle fonction asynchrone (async) appelée `storeEventData`. Cette fonction doit essayer d'obtenir les données de l'événement à partir du corps de la requête, de les stocker et de renvoyer une erreur en cas d'échec.
En cas de stockage réussi, nous retournons le cid qui pointe vers un répertoire IPFS du fichier que nous venons de stocker.

Dans cette fonction, il y a deux fonctions qui seront appelées. La première est une fonction async `makeFileObjects`. Le but de cette fonction est de créer un fichier json qui inclut les métadonnées transmises par l'objet `req.body`. La fonction suivante que nous appelons est la fonction `storeFiles`, qui va stocker ce fichier json dans Web3.storage.

```javascript
async function storeEventData(req, res) {
  const body = req.body;
  try {
    const files = await makeFileObjects(body);
    const cid = await storeFiles(files);
    return res.status(200).json({ success: true, cid: cid });
  } catch (err) {
    return res
      .status(500)
      .json({ error: "Error creating event", success: false });
  }
}
```

Sous notre fonction storeEventData, créez une nouvelle fonction async appelée `makeFileObjects`. Cette fonction va créer un `Buffer` à partir du body stringifié.

Cette fonction va aussi chercher l'image dans `body.image`. Nous pouvons utiliser une fonction de `web3.storage` appelée `getFilesFromPath` pour récupérer l'image dans notre dossier images. Cette fonction renvoie l'image dans un tableau, et nous pouvons pousser notre fichier de données dans ce tableau afin de pouvoir télécharger l'image et les données de l'événement en même temps. Nous utiliserons cette fonction pour créer un nouveau `File` à partir du `buffer` que nous pourrons nommer `"data.json"`, et ensuite le pousser dans le tableau `files`.

```javascript
async function makeFileObjects(body) {
  const buffer = Buffer.from(JSON.stringify(body));

  const imageDirectory = resolve(process.cwd(), `public/images/${body.image}`);
  const files = await getFilesFromPath(imageDirectory);

  files.push(new File([buffer], "data.json"));
  return files;
}
```

Maintenant que nos fichiers sont prêts à être téléchargés, nous voulons d'abord créer un objet client Web3Storage avec lequel interagir. Nous allons écrire la fonction suivante :

```javascript
function makeStorageClient() {
  return new Web3Storage({ token: process.env.WEB3STORAGE_TOKEN });
}
```

Une fois que nous avons créé notre client Web3Storage, nous pouvons appeler la méthode `put` sur le client pour télécharger notre tableau de fichiers.

```javascript
async function storeFiles(files) {
  const client = makeStorageClient();
  const cid = await client.put(files);
  return cid;
}
```

Lorsque nos fichiers ont été téléchargés, `client.put` renvoie un identifiant de contenu (CID). Ce CID est le hash unique que nous allons stocker sur la chaîne et utiliser pour récupérer nos fichiers.

---

Écrivains: [Busayo](https://twitter.com/amoweo), [Sarah Z](https://twitter.com/haegeez),
Rédacteurs: [Sarah Schwartz](https://twitter.com/schwartzswartz)
Traducteur: [CookingCryptos](https://twitter.com/CookingCryptos)
