# Projet session

Projet en 3 remises, en équipe de 3 ou de 4 (3 équipes dans la classe).

 * Début du projet: Lundi 9 février
 * Remise 1 (5%), remise mercredi 18 février
 * Remise 2 (10%), remise vendredi 27 février
 * Remise 3 (35%), remise vendredi 13 mars

Lien github de remise: https://classroom.github.com/a/VRFiHAbX

## Mise en contexte

Votre équipe décide de développer une application web qui sera présentée et vendu à plusieurs restaurants. L'application doit donner un maximum de fonctionnalités utiles et gérer plusieurs niveaux de droits. L'application doit être développée avec la MEAN stack (MongoDB ou PostgreSQL, Express ou NestJS, Angular, Node.js) et publiée sur un serveur web de votre choix. Votre application doit être sécurisée et performante.

À chaque sprint, vous devrez sélectionner les fonctionnalités à développer, vous pouvez modifier le carnet de produit selon les besoins. Comparez-vous avec vos compétiteurs afin de voir si votre progression est adéquate. Imaginez que votre application pourrait être achetée par McDonald's, Benny ou le Bharati.

Exemples de fonctionnalités:

   * Gestion des utilisateurs avec différents rôles (administrateur, gérant de succursale, client);
   * Gestion des succursales (ajout, modification, suppression);
   * Gestion des menus (catégories, éléments de menu avec images, prix, étiquettes);
   * Système de commande en ligne (ajout au panier, choix de livraison ou ramassage, formulaire de paiement);
   * Suivi du statut des commandes en temps réel (on simule ce qui n'est pas dans l'application avec un délais qui peut être aléatoire);
   * Pages statiques pour présenter le restaurant et ses services.

Dans votre cours de projet mobile, vous allez développer une application mobile qui communiquera avec ce serveur web.

## Spécifications techniques

 1. git
    1. Vous devez utiliser le répertoire Git prévu pour le projet dans GitHub.
    2. Votre premier commit doit contenir un document présentant vos normes de programmations, un README.md, un install.md (peut être vide pour l'instant) et votre carnet de produit.
    3. Le code (JavaScript, TypeScript, HTML, CSS, SCSS, etc.) doit être sur git.
    4. Les autres fichiers (images, word, diagrammes, ...) peuvent être réalisés sans git, mais leur version finale devra s'y trouver.
    5. Portez une attention particulière aux commentaires de commit, ils seront corrigés.
    6. Chaque fonctionnalité doit être développée dans une branche dédiée, nommée de façon explicite (ex: feature/gestion-utilisateurs), puis fusionnée dans la branche principale via une Pull Request.
    7. Chaque Pull Request doit être revue et approuvée par au moins un autre membre de l'équipe avant d'être fusionnée.
    8. Chaque commit doit inclure un fichier contenant le prompot qui a généré le code.
    9. La qualité du code du Pull Request sera évaluée, si elle est refusée vous devrez la corriger avant de pouvoir fusionner et vous perdez des points.
    10. Si vous acceptez une Pull Request qui contient du code de mauvaise qualité, vous perdez des points.
 2. MEAN Stack
    1. Votre application doit être développée avec la MEAN stack, vous pouvez changer MongoDB par PostgreSQL ou CouchDB. Vous pouvez remplacer Express par NestJS.
    2. Votre site doit être publié sur un serveur web ou sur un VPS de votre choix à chaque sprint. Le site doit rester en ligne jusqu'à la correction finale (peut aller au 1 avril)
 3. Agile
    1. Il y aura 3 sprints et chacun aura une durée d'environ 4 périodes
    2. Les pratiques SCRUM sont obligatoires, vous devez faire l'analyse avec le poker planning et vous engager à prendre un certain nombre de tâche durant la rencontre de début de sprint.
    3. On aura un daily scrum à chaque période, 15 minutes par équipes, l'ordre sera déterminé à l'avance
    4. Comme le cours est en présentiel, la participation à distance ne sera pas acceptée.
    5. L'utilisation d'un Kanban (Github, Trello ou autre) est obligatoire.
 4. Remises
    1. Les remises doivent être identifiés par une étiquette Git sur la branche principale, le positionnement de l'étiquette est décidé par l'équipe.
    2. Si votre commit brise le commit d'un coéquipier, vous perdez les points.

## Sécurité

Vous devez implémenter la sécurité pour tous les problèmes potentiels suivants, consultez vos notes de Web2 ou le site hacksplaining.com pour plus de détail sur chaque point.

 * Manquement dans le ACL (Broken Access Control)
 * Traversée de dossiers (Directory Traversal)
 * CSRF (Cross-Site Request Forgery)
 * SSL (Unencrypted Communication)
 * Injection SQL (SQL Injection)
 * Injection de commande (Command Execution)
 * XSS, temporaire, permanent et auto-infligé (Cross-site Scripting)
 * Conception fragile (Insecure Design)
 * Fuite d'information (Information Leakage)
 * Téléversement dangereux (File Upload Vulnerabilities)
 * Configuration lousse (Lax Security Settings)
 * Dépendance toxique (Toxic Dependencies)
 * Gestion du mot de passe (Password Mismanagement)
 * Recensement des utilisateurs (User Enumeration)
 * Vol de session (Session Fixation & Weak Session ID)
 * Escalade de privilège (Privilege Escalation)
 * Journalisation (Logging And Monitoring)
 * SSRF (Server-Side Request Forgery)
 * Redirection ouverte (Open Redirects)
 * Détournement de clic (Clickjacking)
 * Cross-Origin Resource Sharing - CORS (pas dans les notes ni sur hacksplaining)

La correction doit être détaillée dans un fichier (md ou autre) sur votre Github. Vous pouvez l'expliquer en liant simplement un commit (si c'était la seule chose ajouté au commit et que la faille est comblé en entier) ou en décrivant:

 * Qui est responsable (nom, ou "responsabilité d'équipe")
 * Fichier précis, ou groupe de fichiers (exemple: dans tous les composants Angular)
 * Explication de ce qui a été fait ou de ce qui doit être fait

## Grille de correction, remises 1 et 2

Compte pour 5\% et 10\% de votre session.

| Critère | Excellent | Bon | À améliorer | Problématique |
| ------- | --------- | --- | ----------- | ------------- |
| Tâches | Déployées, fonctionnelles, sans bug, client content<br>10 | Déployées, fonctionnelles, client content<br>8 | Déployées, fonctionnelles, bug mineur<br>4 | Non déployée ou non fonctionnelle<br>0 |
| Code | Structure et normes respectés<br>10 | Bonne structure mais quelques erreurs de normes<br>7 | Une erreur de structure<br>4 | Structure et normes problématiques<br>0 |

## Grille de correction, remise 3

Compte pour 35\% de votre session.

| Critère | Excellent | Bon | À améliorer | Problématique |
| ------- | --------- | --- | ----------- | ------------- |
| Tâches | Déployées, fonctionnelles, sans bug<br>25 | Déployées, fonctionnelles, bug mineur<br>20 | Déployées, fonctionnelles, bug majeur<br>10 | Non déployée ou non fonctionnelle<br>0 |
| Utilisation de Git | Bons archivages, pull request et branches<br>15 | Quelques mals nommés<br>12 | Plusieurs problèmes<br>5 | Peu d'archivages par remise<br>0 |
| Respect des normes de programmations | Normes respectés<br>5 | Quelques erreurs<br>3 | N.D. | Erreurs courantes<br>0 |
| Apparence et ergonomie | Interface professionnelle*<br>5 | Problème mineur<br>3 | Problème majeur<br>1 | Interface différente du thème ou non-professionnelle<br>0 |
| Sécurité | Pas de faille<br>15 | Faille mineure, sans danger<br>10 | Faille mineure, danger temporaire ou isolé<br>5 | Une faille<br>0 |
| Structure de l'application | MEAN respecté<br>10 | 1 ou 2 fichier mal positionné<br>5 | N.D. | Structure non-respectée<br>0 |
| Performance | Échanges clients/serveur/BD limités<br>5 | Pas d'ajout de performance<br>3 | N.D. | Perte de performance visible<br>0 |
| AI | Prompt claire, efficace et commenté<br>10 | Prompt peu clair, peu efficace ou pas commenté<br>6 | Prompt peu clair, peu efficace et pas commenté<br>3 | Prompt problématique ou pas de prompt<br>0 |
| Présentation | Présentation claire, professionnelle et complète<br>10 | Présentation claire et professionnelle<br>7 | Présentation peu claire ou peu professionnelle<br>4 | Présentation problématique ou absente<br>0 |

 * Interface professionnelle: Bonne utiisation du CSS (avec ou sans cadre CSS), l'utilisateur ne cherche pas les fonctionnalités, A11Y pour les personnes voyantes, site réactif

La note finale sera au pro-rata des tâches réalisés.

### Présentation

Vous devrez présenter votre projet à la classe, les fonctionnalités ainsi qu'un aperçu du code. Plus de détail à venir.

## Grille gestion de projet

Cette évaluation compte pour 20\% de votre session.
Sur les 3 sprints vous êtes évalués sur votre travail d'équipe, chaque sprint vaut un tier des points.

| Critère | Excellent | À améliorer | Problématique |
| ------- | --------- | ----------- | ------------- |
| Poker Planning | Participe activement et de bonne foi<br>10 | Participe passivement <br>5 | Ne participe pas ou manque de rigueur<br>0 |
| Daily scrum | Participe activement à chaque rencontre<br>5 | Manque 1 rencontre<br>3 | Manque plus de 1 rencontre<br>0 |
| Professionnalisme | Respect, aide les autres si demandé, présent aux rencontres d'équipes autres que Scrum<br>5 | Respect, manque de présence dans l'équipe<br>2 | Manque de respect ou n'aide pas l'équipe<br>0 |

Une évaluation des pairs m'aidera avec l'évaluation de ce critère.