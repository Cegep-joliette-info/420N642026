# MEAN

Dans ce cours nous allons utiliser la "MEAN Stack". MEAN:

 - MongoDB - Votre BD en No-SQL, gère du JSON au lieu de gérer des relations
 - Express - Le serveur qui fournira l'API qui se connecte à Manjaro
 - Angular - Cadre d'application JavaScript qui fera le "frontend", communique avec Express via HTTP
 - Node.js - Utilisé par les autres éléments de la pile MEAN

Ce qu'il y a d'intéressant avec cette pile?

Un petit rappel pour commencer:
JSON: JavaScript Object Notation, donc du JSON représente un objet JavaScript.

 1. Angular crée un objet JS basé sur les actions de l'utilisateur
 2. Il transforme cet objet en JSON et l'envoie à Express via HTTP
 3. Express reçoit le JSON, le transforme en objet et le traite
 4. Express retransforme l'objet en JSON et l'envoie à MongoDB qui le stock telquel

On travail donc tout le temps avec du JavaScript/JSON. Comme le JSON fait partie du JavaScript, lorsqu'on transforme en JSON ou l'inverse, le coût d'exécution est vraiment faible en JS comparé aux autres langages. Même que ça [coûte moins cher en JS d'utiliser le JSON que de créer un nouvel objet](https://v8.dev/blog/cost-of-javascript-2019#json).

Nous allons travailler avec TS (TypeScript) au lieu de JS (JavaScript). TS est un JS++, c'est du JavaScript typé avec quelques ajouts.

Je n'ai pas trouvé de sources fiables, mais la plupart des sites affichent node.js comme 2 fois plus rapide que PHP (mais node.js reste plus lent que Go ;) ). Alors pourquoi est-ce qu'on a vu PHP au dernier cours?

 1. L'hébergement PHP/MySql est courant et vraiment pas cher comparé à l'hébergement node.js/MongoDB.
 2. PHP est plus populaire, il y a plus de documentation et de questions sur StackOverflow.
 3. Node.js est "[single-thread](https://medium.com/better-programming/is-node-js-really-single-threaded-7ea59bcc8d64)". Donc si vous faites une action qui demande beaucoup de processus vous bloquez votre site pour tous les utilisateurs (génération de PDF ou de Excel, traitement d'image, gestion de fichiers, etc.).

Pour ces raisons, il y a même une entreprise qui utilise 2 API sur ces sites: 1 API node.js pour les appels CRUD simples et 1 API PHP pour les appels lourds pour le processeurs.