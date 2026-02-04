# Atelier

Travail individuel. Compte pour 10% de votre session. Lien GitHub Classroom pour la remise: https://classroom.github.com/a/OiC1Ixq0

Date de remise: Vendredi 13 février 2026 12h50


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
 - Vous devez utiliser un framework CSS ou un thème CSS (tailwind/material)
   - L'interface doit être réactive (responsive) et accessible (WCAG AA minimum)
 - Chaque action dans l'interface doit fournir un retour immédiat et clair
 - Sécurité à considérer pour l'atelier:
   - S'assurer que je ne crée pas plusieurs articles si je spam le bouton (idem pour toutes les actions)
   - XSS temporaire et permanent
   - CSRF
   - Hashage du mot de passe dans la base de donnée
   - Empêcher/cacher le HTML non-disponible en Angular
   - Protéger le CRUD, les autres ne doivent pas modifier ou supprimer mes articles

AI:
 - Incluez le ou les prompts utilisés dans un dossier ai-prompts, chaque prompt dans un fichier texte séparé ordonnée numériquement ou avec la date
 - Ajoutez un document README.md dans le dossier ai-prompts expliquant ce que vous avez fait après chaque prompt, par exemple: "L'ai a créé Angular dans le mauvais dossier je l'ai donc déplacé" ou "L'ai n'a pas respecté les normes de code j'ai donc corrigé manuellement"
 
Correction:
 - Foncitonnalités du site - /2
 - Qualité du code - /2
 - Sécurité du CRUD - /2
 - Documentation des prompts AI - /4

Pour avoir tous vos points, votre code doit:
 - Respecter les normes de programmations Angular et JavaScript
 - Bien fonctionner sans effet de bord (pas de bugs)
 - Votre note ne peut pas dépasser le nombre de fonctionnalités complétées (Si vous faites la moitié parfaitement, vous aurez 50%)