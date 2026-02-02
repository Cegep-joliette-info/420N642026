# Déploiement

Cette procédure vous donne la base, il va rester des petits bugs à régler par la suite.

## Préparation

Sur votre serveur, commencez par le mettre à jour:

```
sudo apt update
```

Installez ensuite:

 * Apache2 `sudo apt install apache2`
 * Mongodb, suivez [la procédure](https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-ubuntu/)
 * Node.js, roulez les commandes pour Node.js dernière version avec un nombre PAIR, pour Ubuntu. [Nodesource](https://github.com/nodesource/distributions?tab=readme-ov-file#installation-instructions)
 * PM2, roulez la commande `npm install pm2 -g`

## Frontend (Angular)

Sur votre ordinateur, dans votre dossier WWW, roulez la commande: `npm run build`.

Copiez le contenu du dossier créé (`/WWW/dist/nomduprojet`) dans le dossier web du serveur (`/var/www/html`).

Entrez l'adresse IP de votre serveur dans un navigateur, vous devriez voir votre site.

## Backend (Express)

Sur le serveur, créez un dossier pour accueillir l'API. Ce dossier doit être en dehors du dossier d'apache (`/var/www`).

Dans votre API, modifiez le fichier `package.json` pour ajouter le script `"build": "tsc"`, ce qui donne:

```json
{
  "name": "api",
  "version": "0.0.0",
  "scripts": {
    "start": "nodemon index.ts",
    "build": "tsc"
  }
}
```

Il faut aussi mettre à jour le `tsconfig.json` pour activer le `"outDir"` et mettre le dossier de compilation à "dist", ce qui devrait donner:

```
"outDir": "./dist",
```

Roulez la commande build (avec la flèche sur WebStorm ou avec la commande `npm run build`), ça va créer un dossier dist qui contient votre API en JS (ça a convertis le TS en JS).

Copiez le contenu du dossier dist ET le dossier node_modules sur le serveur.

Démarez votre API avec pm2. `pm2 start index.js`

Dans apache, configurez un proxy de `/api` vers le port express (3000). Dans votre vhost (`/etc/apache2/sites-available/default.conf`), ajoutez le proxy:

```
ProxyPass /api/ "http://localhost:3000/"
```