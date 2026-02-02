# Routes

On a sortie notre logique de notre index.ts pour la mettre dans des contrôleurs, c'est bien, mais nous avons tout de même plusieurs routes dans notre index.ts. Notre fichier index devrait seulement 'bootstrap' notre application. On va donc sortir nos routes dans d'autres fichiers. Je me suis créé un dossier 'routes' et à l'intérieur on va créer 2 fichiers:

```js
// usersRoutes.ts
import express from "express";
import { UsersController } from '../controllers/usersController';
let router = express.Router();
router.get('/', getAllUsers);
export { router };

// routes.ts
import express, { Request, Response } from "express";
import { router as usersRouter } from "./usersRoutes";
let router = express.Router();
router.use('/users', usersRouter);
export { router };
```

usersRoutes.js rassemble toutes les routes pour les users et le fichier routes.js va rassembler tous les fichiers de routes. Ces deux fichiers ont une structure similaire: Importations, Routes et Exportations. Avec cette configuration, le 'getAllUsers' sera appelé avec l'URL '/users'.

Finalement, on retourne dans le index.ts, on importe notre fichier routes.ts et on remplace les lignes de routes par:
```js
import { router } from "./routes/routes";
app.use('/', router);
```

## Verbes HTTP

 - get, pour recevoir de l'information
 - post, pour créer une resource
 - put, pour modifier une resource
 - delete, pour supprimer une resource