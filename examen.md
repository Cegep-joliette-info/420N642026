# Examen - Version préliminaire

Travail individuel. Compte pour 10\% de votre session. Lien GitHub Classroom pour la remise: à venir

Date de remise: à venir


Vous devez faire un système de blogue très simpliste.

Un article contient:
 - Un titre
 - Un message texte (pas de HTML)
 - Une date de création
    - La date est affichée avec la plus petite échelle entière possible (il y a 3 secondes, minutes, heures, jours, semaines, mois, années)

Un utilisateur anonyme peut:
 - Lire tous les messages
 - Se connecter et se créer un compte

Un utilisateur connecté peut:
 - Se déconnecter
 - CRUD de ses messages à lui

Autres détail:
 - La base de données doit être en PostgreSQL
 - Vous devez utiliser un framework CSS ou un thème CSS
 - Vous pouvez seulement utiliser les paquets npm vus dans le cours
 - Chaque action dans l'interface doit fournir un retour immédiat et clair
 - Sécurité à considérer pour l'atelier:
   - S'assurer que je ne crée pas plusieurs articles si je spam le bouton (idem pour toutes les actions)
   - XSS temporaire et permanent
   - CSRF
   - Hashage du mot de passe dans la base de donnée
   - Empêcher/cacher le HTML non-disponible en Angular
   - Protéger le CRUD, les autres ne doivent pas modifier ou supprimer mes articles
 
Correction:
 à venir

Pour avoir tous vos points, votre code doit:
 - Respecter les normes de programmations Angular et JavaScript
 - Bien fonctionner sans effet de bord (pas de bugs)