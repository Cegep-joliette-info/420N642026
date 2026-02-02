# Médiateur (*middleware*)

Un *middleware* permet de simplifier encore plus le travail de vos contrôleurs.
On va l'utiliser surtout pour la sécurité et la réutilisation de code.

La structure est similaire à un contrôleur, par exemple (exemple non fonctionnel), avant on avait:

```js
router.get('/', getAllUsers);

function getAllUsers(req, res) {
  if (!isAdmin()) {
    res.sendStatus(403);
  }
  let users = *Requête DB ici*
  res.json(users);
}
```

La même chose avec un *middleware*:

```js
router.get('/', isAdmin, getAllUsers);

function isAdmin(req, res, next) {
  if (!isAdmin()) {
    res.sendStatus(403);
  }
  else {
    next();
  }
}

function getAllUsers(req, res) {
  let users = *Requête DB ici*
  res.json(users);
}
```

*isAdmin* est le *middleware*. On respecte mieux le principe de responsabilité unique, *isAdmin* valide le rôle de l'utilisateur tandis que *getAllUsers* va seulement chercher les utilisateurs.

Dans la route, il y a deux fonctions. Express va appeler la 2e seulement si la première appel la fonction next. C'est une belle application du [patron de conception *chain of responsability*](https://refactoring.guru/design-patterns/chain-of-responsibility).

Vous pouvez même faire des *middleware* de validation, comme ça le contrôleur devient encore plus simple et vous pouvez réutiliser la validation pour les actions de création et de modification.

Vous pouvez passer de l'information au *middleware* suivant ou au contrôleur en modifiant l'objet res. Par exemple je pourrais envoyer un user avec `res.locals.user = user;` et dans le contrôleur je pourrai accéder à `res.locals.user`. Ce petit truc peut éviter de faire plusieurs requêtes BD similaires dans une chaîne de *middlewares*.