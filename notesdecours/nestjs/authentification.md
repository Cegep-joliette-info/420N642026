# Authentification

On va utiliser Passport pour l'authentification dans NestJS. C'est la bibliothèque recommandée par NestJS pour gérer l'authentification. On va utiliser la stratégie locale (username/password) pour cet exemple.

Je ne peux pas mieux faire que la doc officiel pour la configuration: https://docs.nestjs.com/recipes/passport

Faites attention à:

 - Bien lire et comprendre le code que vous copiez-collez de la documentation officielle.
 - Bien incorporer `bcrypt` pour le hachage des mots de passe, bcrypt devrait seulement être utilisé par le module auth, le module user ne devrait pas savoir que les mots de passe sont hachés.
 - Vous ne pourrez pas tester le login sans avoir un utilisateur dans la base de données (le tutoriel utilise Curl). Donc à cette étape générez le `register`.