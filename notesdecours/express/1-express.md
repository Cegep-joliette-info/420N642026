# Express

Express est un serveur simple Node.js. C'est lui qui va recevoir les appels d'API et retourner le JSON sur le réseau, ainsi que communiquer
avec la base de données.

On va modifier notre projet dans Webstorm, on va déplacer tout notre code Angular actuel dans un dossier "client" et nous allons créer
un dossier "server". On pourrait aussi utiliser les termes "WWW" et "API".

Dans votre dossier server on va initialiser npm avec la commande `npm init`, les réponses ne sont pas importantes. Modifiez ensuite le fichier package.json, gardez seulement les attributs name et version.

Installer les dépendances suivantes avec la commande `npm install` suivit de toutes les dépendances séparés par des espaces:
 - body-parser : Sert à lire le body dans les appels API (pour les POST)
 - cors : Ce nom qui fait faire des cauchemars, on doit l'installer...
 - express : Notre serveur!
 - express-validator : Pour faire de la validation facilement (required, minLength, isEmail, etc.), même si on valide côté client il faut valider côté serveur!
 - mongoose : Le SQL ça fait 1990, on touche pas à ça, surtout que Mongo ne supporte pas SQL, c'est du NoSQL ;)
 - bcryptjs : Pour encrypter les mots de passes
 - jsonwebtoken : Pour créer et valider des JWT facilement
 - cookie-parser : Lire et créer des cookies, cookie monster approves

Pour la BD, ajoutez la dépendance correspondante:
 - mongoose: Pour MongoDB
 - pg-promise: Pour PostgreSQL
 - nano: Pour CouchDB
 - Pour les autres, consultez la page https://expressjs.com/en/guide/database-integration.html

Et les dépendances de dev en ajoutant `--save-dev` au `npm install`:
 - @types/body-parser: Activer l'autocomplétion pour cette dépendance
 - @types/cookie-parser: Idem
 - @types/cors: Idem
 - @types/express: Idem
 - @types/node: Idem
 - @types/jsonwebtoken: Idem
 - typescript : Permet d'utiliser TypeScript
 - ts-node : Permet à Node.js d'exécuter du TS
 - nodemon: Iä! Iä! Cthulu fhtagn! (restart le server s'il plante ou est modifié, installation globale (-g) sur un serveur)

## Configuration TypeScript

Exécutez la commande `npx tsc --init` qui va créer un fichier tsconfig.json qui contient toutes les options de configurations de TypeScript.

Créez aussi un fichier `.gitignore` qui contient la ligne suivante:

```
node_modules
```

Finalement, ajoutez le script nodemon à votre package.json, qui devrait ressembler à ça:

```
{
  "name": "api",
  "version": "0.0.0",
  "scripts": {
    "start": "nodemon index.ts"
  },
  "dependencies": {...}
  "devDependencies": {...}
}
```

## Configuration de base

Créez un fichier 'index.ts'. Voici le fichier:

```ts
import express, { Express, Request, Response } from "express";

const app: Express = express();
app.get('/', function(req: Request, res: Response): void {
    res.send('ok');
});
app.listen(3000, () => {
    console.log('listening to post 3000');
});
```

Les deux premières lignes importent et créent l'objet express qui contient le serveur. Le 'app.get' est le routage simple, on attends un appel GET sur la racine de l'API.
Les paramètres sont req (la requête) et res (la réponse).
On peut utiliser 'res' pour envoyer n'importe quel type JavaScript.
Finalement, le app.listen démarre le serveur sur le port 3000, le console.log est simplement pour s'assurer que le serveur est bien démarré.

## Récupérer le body

Les données envoyés en PUT ou POST sont envoyés dans le body en JSON. Pour récupérer le JSON il faut ajouter à notre index.ts, entre l'instanciation du serveur express et nos routes, ajouter le code suivant:

```ts
// import à mettre en haut du fichier
import bodyParser from "body-parser";

// ligne à mettre en le express et les routes
app.use(bodyParser.json());
```

Le `app.use` ajoute un middleware à notre application, du code exécuté entre la réception de la requête et l'appel à notre contrôleur. Le `bodyParser.json` récupère le body texte/json, le transforme en objet javascript et le met dans la requête.

Vos fonctions de routes utilisent un paramètre req de type Request, avec `req.body` on récupère l'objet envoyé par Angular. 

## Codes HTTP

Par défaut Express retourne des codes 200 (OK, avec réponse).
Par contre, on va parfois avoir des erreurs ou aucune réponse, il faut donc changer le code HTTP de la réponse.
Au lieu de retourner une réponse avec:

```js
res.send('bla');
```

On va utiliser parfois:

```js
res.status(200).send('bla');
```

Voici la liste des codes HTTP avec leur signification (seulement les codes intéressants selon moi):

 - 1XX: Les codes 100 à 199 signalent qu'on a reçu la requête mais que la réponse n'est pas prête, pas utile dans notre cas
 - 2XX: Les codes 200 à 299 signalent que la requête a été acceptée
   - 200: OK, Requête accepté et retourne une réponse
   - 201: Created, Une nouvelle ressource a été créé, la ressource créé est habituellement retourné (surtout pour avoir l'ID)
   - 202: Accepted, Requête acceptée mais non traitée, pourrait même ne pas fonctionner
   - 204: No Content, Requête accepté mais ne retourne rien (sur un update par exemple)
   - 205: Reset Content, Signale au client de rafraichir la vue
 - 3XX: Les codes 300 à 399 signalent une redirection, je ne pense pas que ça va être utile dans notre cas
 - 4XX: Les codes 400 à 499 signalent une erreur du client, ces codes seront très utiles
   - 400: Bad Request, la route existe mais les données ne sont pas bien formés (il manque un champ, erreur de type, etc.)
   - 401: Unauthorized, connecte toi stp
   - 403: Forbidden, hack ton JWT parce que tu n'as pas le droit d'accéder à cette route
   - 404: Not Found, la route n'existe pas
   - 418: I'm a teapot, Le serveur refuse d'infuser du café avec une théière (ce n'est pas une blague, ce code veut vraiment dire ça)
 - 5XX: Les codes 500 à 599 signalent une erreur serveur, un service indisponible, une méthode 'Not implemented', etc.

Pour la liste complète, voir https://developer.mozilla.org/fr/docs/Web/HTTP/Status

On va utiliser les codes 4XX pour retourner toute erreur à Angular (non-authorisé, formulaire mal remplis, bug, etc.).