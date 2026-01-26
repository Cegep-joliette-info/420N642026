# Modules

> **Note:** Avec les versions récentes d'Angular (14+), les modules ne sont plus obligatoires grâce aux composants standalone. Cependant, ils restent utiles pour organiser des applications complexes. Par exemple, un module `admin` regroupe tous les composants, services et routes d'administration; un module `shared` centralise les composants réutilisables; un module `feature` isole une fonctionnalité métier avec son propre routing et lazy-loading.


Les modules permettent de structurer notre code en plusieurs sections (modules). Par exemple, on pourrait avoir un module d'administration et un module de gestion d'inventaire dans une application de vente. Par défaut vous avez déjà un module: App. Ce module va contenir toute votre application. Pour générer un module, entrez une des deux commandes suivantes:

```
ng generate module admin
ng g m admin
```

Ensuite, si je veux créer un composant dans ce module, il suffit de mettre le nom du module avant le nom du composant:

```
ng g c admin/login
```

Cette commande va créer le composant login dans le module admin. Par défaut, tous les modules sont chargés en mémoire lorsqu'on charge l'application. Ça peut être pratique lorsqu'on a un module 'shared', mais moins pratique pour notre module 'admin' qui sera utilisé par peu d'utilisateurs. On va donc utiliser le 'lazy-loading', pour activer cette fonctionnalité il faut créer un module avec du 'routing':

```
ng g m admin --route admin --module app.module
```

La partie '--route admin' va activer le routing dans le module admin et va modifier le 'app-routing.module.ts', toutes les routes du module admin vont débuter par '/admin'. Le '--module app.module' vient dire que le module admin est un sous-module du module App. Le module admin peut maintenant avoir ses composantes, ses routes, ses services etc.

Si vous voulez utiliser un composant qui se trouve dans un autre module, vous devez exports le composant dans le module (admin.module.ts, dans le décorateur @NgModule) afin de pouvoir l'import dans le deuxième module. Notez que si vous faites ça avec un service, une deuxième instance de ce service sera créé.