# Garde Angular

Un garde Angular est un principe qui permet de sécuriser des routes. Premièrement, on va créer un service d'autorisation, ce service sera utilisé par le garde et vos vues pour afficher/cacher des éléments HTML. Dans ce service je vais mettre la fonction suivante:

```ts
isLoggedIn(): boolean {
  let user = localStorage.getItem('user');
  return user != null;
}
```

Notez que dans ce cas le localStorage doit être remplis à la connexion utilisateur.

Maintenant on va créer le garde avec la commande suivante:

```
ng g guard auth
```

La ligne de commande va vous demander quelle fonction vous voulez implémenter (1 seule par fichier). Chaque fonction active une fonctionnalité différente:

 * canActivate: Détermine si la route est accessible, ne sera pas appelé si on passe de cette page vers une page enfant, mais sera utilisé si on accède directement à l'enfant
 * canActivateChild: Détermine si les enfants de cette route sont accessibles, ne sera pas appelé pour cette route mais sera appelé pour tous les enfants
 * canDeactivate: Peut être utilisé pour empêcher un utilisateur de quitter une page, utile lorsqu'un formulaire est modifié mais non sauvegardé
 * canMatch: Très similaire à CanActivate, mais ne charge pas le contenu si l'accès est refusé, utile pour le module Admin par exemple.

Votre fonction peut retourner un booléen ou un lien. Si une route est protégé par plusieurs guardes, tous doivent retourner true pour avoir accès.

Comme ce n'est pas une classe, on ne peut pas injecter les dépendances via le constructeur, il faut les injecter manuellement (exemple de: https://angular.io/guide/router-tutorial-toh#canactivate-requiring-authentication):

```ts
import { inject } from '@angular/core';
import { Router } from '@angular/router';
import { AuthService } from './auth.service';

export const authGuard = () => {
  const authService = inject(AuthService);
  const router = inject(Router);

  if (authService.isLoggedIn) {
    return true;
  }

  // Redirect to the login page
  return router.parseUrl('/login');
};
```

Finalement, on ajoute le garde à notre router:

```ts
const routes: Routes = [
  { path: 'create', component: CreateComponent, canActivate: [AuthGuard] }
];
```