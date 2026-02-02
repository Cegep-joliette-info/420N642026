# Contrôleur

Pour l'instant notre code est entièrement dans le fichier index.ts. À long terme ça va créer un problème de complexité, on va donc enlever la logique de traitement des requêtes du index.ts et la mettre dans des contrôleurs.

Un contrôleur peut être un objet, ou simplement un fichier qui contient des fonctions.

## Solution classe (ma pref)

Il faut commencer par importer les types Request et Response:

```ts
import { Request, Response } from "express";
```

Ensuite on déclare et exporte la classe d'un coup:

```ts
export class RickController {}
```

Ensuite, on fait nos fonctions. On les mets statiques puisque sinon, il faut déclarer 1 seule instance de tous les contrôleurs ce qui revient presque au même côté mémoire.

```ts
static justDoIt(req: Request, res: Response) {
    // TODO
}
```

Finalement, on importe et appel le controleur dans notre index.ts:

```ts
import { RickController } from "./controllers/rickController";
app.get('/register', RickController.justDoIt);
```

## Solution fonctions (desuet)

Premièrement on crée notre fonction qui va prendre la requête et renvoyer la réponse:

```js
function getAllUsers(req, res) {
  res.json([]);
}
```

Et à la fin du fichier il faut exporter notre classe ou nos fonctions:

```js
export{getAllUsers};
```

Notez qu'il faudra importer des modules comme celui ci:

```js
import { Response } from 'express';
```

Maintenant on importe notre contrôleur dans notre index.ts et on change la route:

```js
app.get('/users', getAllUsers);
```