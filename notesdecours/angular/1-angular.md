# Angular

Angular est un cadre d'application développé par Google. Il s'agit d'une nouvelle version de AngularJS, mais cette nouvelle version est plaisante à utiliser.

Angular est structuré en modules, composants (components) et services.

### Module

**Encore accepté, mais ne doit plus être utilisé**

Angular a un module par défaut, le module racine. Vous pouvez créez d'autres modules afin de regrouper des éléments, par exemple le module Admin et le module de Connexion. Notez qu'Angular charge tout votre programme en mémoire, mais permet de charger seulement un module à la fois.

### Composant

Un composant gère tout ce qui est lié fortement à une vue. Chaque composant a donc son contrôleur, son HTML et son CSS. Une vue peut être une page entière ou tout petit tel que le bouton de déconnexion. Un composant peut inclure d'autres composants.

### Service

Un service est tout élément qui n'est pas lié fortement à une vue: les requêtes Fetch, journalisation des événements, gestion des
messages d'alertes, etc.

![Schéma Angular](../../images/angular-overview2.png)

Pour plus de détail: https://angular.io/guide/architecture

## Normes de programmation

En détail sur cette page: https://angular.io/guide/styleguide

Je vais résumer les règles les plus importantes ici

### Responsabilité unique

Chaque fichier ne doit contenir qu'un concept (composant, service, modèle, etc.) par fichier. Un fichier ne peut pas avoir plus de 400 lignes.

N.B. Retirer de l'aération verticale pour avoir moins de ligne n'est pas une solution acceptable.

Vos fonctions doivent être courtes, 75 lignes maximum.

Pourquoi une limite sur la longueur des fichiers et fonctions? Une fonction (ou fichier) plus courte est plus rapide/facile à comprendre, plus facile à tester, encourage la réutilisation et limite les bugs.

### Nommage

Les fichiers doivent avoir le nom <fonctionnalité>.<type>.ts. Donc le composant de login aurait le nom: "login.component.ts".

Pour séparer les mots dans un nom de fonctionnalité, utilisez les '-'. Donc pour le composant de liste d'utilisateurs on aurait: "userslist.component.ts".

Ne pas utiliser d'abréviations.

Le nom des classes (et autres éléments) doit être en notation Pascal (upper camel-case). Le fichier "users-list.component.ts" contiendra la classe "UsersListComponent".

Exception: les services qui ont un verbe comme nom, "logger.service.ts" peut contenir la classe "Logger" au lieu de "LoggerService".

### Structure

Respecter le LIFT, votre application doit être conçu de manière à:

 * (Locate code quickly) Trouver rapidement le code
 * (Identify the code at a glance) Identifier le code d'un coup d'oeil
 * (keep the Flattest structure you can) Garder une structure plate, mais seulement 7 fichiers par dossier maximum
 * (Try to be DRY) Essayer d'être DRY (Don't Repeat Yourself)

## Installation

Assurez vous d'avoir Node.js avec npm d'installé. Pour valider, écrivez la commande suivante dans la console:

```
npm
```

Si une aide est affiché, tout est bien installé, si la commande n'est pas trouvé vérifiez votre installation de Node.js. Pour installer Angular exécutez la commande suivante:

```
npm install -g @angular/cli
```

Le "-g" installe angular globalement, sans le "-g" angular fonctionnera dans le dossier courant seulement. Pour valider qu'Angular est bien installé, exécutez la commande suivante:

```
ng
```

Si une aide est affiché, tout est bien installé. Sinon, il y a surement eu une erreur ou redémarrez votre console/session. Maintenant vous pouvez initialiser votre application angular. Allez dans le dossier parent de votre futur projet et exécutez la commande suivante:

```
ng new
```

Voici un exemple de la sortie de ng new:

```
$ ng new
✔ What name would you like to use for the new workspace and initial project? demo-mean
✔ Which stylesheet system would you like to use? Tailwind CSS    [ https://tailwindcss.com ]
✔ Do you want to enable Server-Side Rendering (SSR) and Static Site Generation (SSG/Prerendering)? No
✔ Which AI tools do you want to configure with Angular best practices? https://angular.dev/ai/develop-with-ai None,
 GitHub Copilot [ https://code.visualstudio.com/docs/copilot/copilot-customization ]
CREATE demo-mean/README.md (1461 bytes)
...
```

La première question demande le nom du projet. Il va créer un dossier avec ce nom.

Pour le CSS, vous pouvez choisir celui que vous voulez. Les choix sont:

 * CSS - Standard
 * Tailwind CSS - Bibliothèque utilitaire CSS la plus utilisée
 * Sass (SCSS) - Préprocesseur CSS le plus utilisé
 * Sass (Indented) - Variante de Sass avec indentation au lieu des accolades 
 * Less - Préprocesseur CSS moins utilisé

Le SSR est le Server Side Rendering. S'il est désactivé, votre serveur contient seulement un HTML et un ou plusieurs JavaScript, ce qui le rend compatible avec Apache ou Nginx. S'il est activé, la page initiale devra être compilé par le serveur, vous avec donc besoin d'un serveur node.js afin de faire fonctionner votre page. Activer le SSR augmente la rapidité du site, améliore le SEO (référencement Google) mais complexifie un peu le déloiement du site.

Le choix des AI tools est optionnel, choisissez celui que vous utilisez.

Le `ng new` possède plusieurs options détaillés ici: https://angular.dev/cli/new

### Génération de code

Ensuite pour créer les composants, services et modules, nous allons encore utiliser la console. A la racine de votre application, exécutez la commande suivante:

```
ng generate component login
```

Vous pouvez utiliser "g" au lieu de "generate". Vous pouvez utiliser "c" au lieu de "component".

```
ng g c login
```

Vous pouvez ajouter des dossiers à "login" afin de créer des sous-dossiers. Dans l'exemple suivant on crée le composant login dans le dossier auth.

```
ng g c auth/login
```

Si vous avez plusieurs modules, vous devez spécifier le module à utiliser avec le flag "--module=app".

Voici les éléments pouvant être générés avec leur raccourcis:

 * component - c
 * service - s
 * enum - e
 * module - m
 * classe - cl
 * interface - i

DigitalOcean offre un beau résumé de toutes les commandes: https://www.digitalocean.com/community/tutorials/angular-angular-clireference.

### Exécution/compilation

Deux commandes importantes:

```
ng serve
ng build
```

Le serve va partir un "serveur" sur le port 4200 (http://localhost:4200). Si vous modifiez un fichier, Angular va automatiquement recompiler tant que le "serve" est actif.

Le build va créer un nouveau dossier avec l'environnement "prod". Les fichiers TS et SCSS sont compilés et minimisés, prêts à être déployés.

Ces commandes sont détaillés dans le fichier `package.json`.

## Material

Bien que vous pouvez utiliser la librairie CSS de votre choix, Angular est fait pour fonctionner avec Material. Pour installer Material exécutez le code suivant à la racine de votre projet Angular:

```
ng add @angular/material
```

La commande va installer les dépendances NPM et modifier vos fichiers pour utiliser le CSS et les fonts de Material.

Lors de l'installation il va vous demander 1 question: le thème à utiliser. Vous pouvez les tester sur le site https://material.angular.io (pot de peinture en haut à droite).

Allez ensuite dans le fichier `material-theme.scss` généré et modifiez les couleurs et le `color-scheme` si nécessaire.

## index.html

 Dans votre index.html vous devez faire la modification suivante:

  * Changer la langue de la page pour français `fr-CA`