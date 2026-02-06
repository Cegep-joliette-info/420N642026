# Prompt

Si vous ne voulez pas payer pour un AI, vous pouvez utiliser GitHub Copilot, qui est gratuit pour les étudiants. Pour avoir la licence gratuite, remplissez ce formulaire : https://education.github.com/pack. Vous aurez besoin d'une preuve d'étude, soit votre carte étudiante ou votre horaire.

Vous devrez me dire quel AI vous utilisez, si vous utilisez un AI plus performant, je m'attends à des prompts plus complexes et des résultats plus précis. Si vous utilisez un AI moins performant, je m'attends à des prompts plus simples. Donc si quelqu'un utilise Claude Max, je m'attends à plus de fonctionnalités développées que quelqu'un qui utilise GitHub Copilot.

## Agents.md

Le fichier agents.md permet de donner des instructions générales à l'AI. Par exemple, vous pouvez lui dire de toujours répondre en français, ou de toujours générer des tests. Voici quelques exemples d'instructions que vous pouvez donner :

 * Toujours générer des tests unitaires pour le code que tu écris, même si je ne te le demande pas.
 * L'interface utilisateur doit toujours être en français.
 * L'interface utilisateur doit toujours utiliser Tailwind CSS, être réactive et accessible (norme WCAG 2).
 * Le code doit toujours suivre les meilleures pratiques de sécurité.
 * Le code doit suivre les normes de codages définies dans le fichier CONTRIBUTING.md.

## Mode plan

Commencez par essayer votre prompt en mode agent, si vous êtes satisfait vous pouvez commit le code. Si le code n'est pas satisfaisant, essayez d'améliorer votre prompt. Si vous n'arrivez pas à obtenir un bon résultat, essayez le mode plan.

Le mode plan permet de diviser le travail en plusieurs étapes. Par exemple, vous pouvez demander à l'AI de générer un plan pour une fonctionnalité, puis de générer le code pour chaque étape du plan. Attention, avec Github Copilot, il exécute toutes les étapes sans pause, ce qui ne donne pas le temps de valider chaque étape. Prenez donc les étapes qu'il donne et exécutez-les une par une.

## Gabarit d'entreprise

Le gabarit fournit dans le fichier exemple.md est un bon point de départ pour créer des prompts efficaces. Mais certains sites préconisent des prompts plus simples. Faites des tests, le plus important est d'obtenir un bon résultat. Vos premiers prompts prendront probablement plusieurs itérations avant d'être satisfaisants.

## Pour le projet

Créez un dossier `Prompts` dans votre projet, vous pouvez faire un sous-dossier par étudiant(e) si vous voulez. Chaque prompt doit être dans un fichier markdown, le nom du fichier doit débuter par le numéro de la tâche ou la date, par exemple `01-login.md`, `02-register.md` ou `20260230-11h30-dashboard.md`

Vous devez fournir le prompt que vous avez utilisé pour générer le code, vous pouvez aussi inclure ce que l'AI vous a fournis comme code, mais ce dernier point n'est pas obligatoire. Ajoutez une section dans le fichier pour me dire ce que vous avez modifié à bras, ou une petite description de ce que vous avez fait pour en arriver au résultat final. Par exemple si vous avez essayé plusieurs types de prompts, dites le moi ça pourrait expliquer pourquoi la tâche a pris plus de temps que prévu.