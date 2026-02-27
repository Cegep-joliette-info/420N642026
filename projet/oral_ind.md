# Oral individuel — Compréhension du code projet

**Cours :** Développement d'applications web  
**Pondération :** 20% de la note finale  
**Durée :** 15 minutes par étudiant  
**Stack évaluée :** PostgreSQL · NestJS · Angular

---

## 1. Contexte et objectif

Durant ce projet, vous avez eu recours à l'intelligence artificielle pour générer une partie significative du code de votre application. Cet entretien vise à s'assurer que vous **comprenez réellement** le code que vous avez livré, que vous êtes capable de l'**expliquer**, de l'**analyser** et d'identifier ses **forces et limites**.

Il ne s'agit pas de mémoriser des définitions, mais de démontrer une compréhension fonctionnelle et critique du code présent dans votre dépôt Git.

---

## 2. Déroulement de l'entretien

| Phase | Durée | Description |
|---|---|---|
| **Introduction** | 1 min | L'enseignant présente 2–3 extraits de code tirés de vos commits |
| **Questions sur le code** | 10 min | Questions ciblées sur les extraits choisis (voir section 3) |
| **Question de recul** | 3 min | Une question de synthèse sur vos choix techniques ou l'utilisation de l'IA |
| **Conclusion** | 1 min | Remarques finales, échange |

> **Note importante :** Les extraits de code présentés proviennent exclusivement de **vos propres commits** dans le dépôt Git du projet. Vous pouvez demander à revoir un extrait si nécessaire.

---

## 3. Types de questions posées

Les questions sont générées à partir de votre code réel et couvrent les catégories suivantes :

### 3.1 Compréhension du code (ce que ça fait)
- « Que fait cette fonction ? Quel est son rôle dans l'application ? »
- « Pourquoi ce bloc de code est-il nécessaire ici ? »
- « Que se passerait-il si on retirait cette ligne ? »

### 3.2 Concepts techniques liés à la stack
- **PostgreSQL :** Relations entre tables, contraintes, requêtes SQL générées par l'ORM, indexes
- **NestJS :** Décorateurs (`@Controller`, `@Injectable`, `@Module`), injection de dépendances, guards, DTOs, pipes de validation
- **Angular :** Composants, services, directives, observables RxJS, routing, binding

### 3.3 Analyse et esprit critique
- « Est-ce que ce code pourrait poser des problèmes en production ? »
- « Comment amélioreriez-vous cette partie ? »
- « Y a-t-il des cas limites que ce code ne gère pas ? »

### 3.4 Utilisation de l'IA (recul professionnel)
- « Comment avez-vous vérifié que le code généré était correct ? »
- « Y a-t-il des parties que l'IA a mal générées et que vous avez dû corriger ? »
- « Qu'est-ce que vous avez appris en travaillant avec l'IA sur ce projet ? »

---

## 4. Ce qui est évalué

L'entretien évalue **quatre dimensions** :

1. **Compréhension fonctionnelle** — Vous savez ce que le code fait et pourquoi
2. **Maîtrise des concepts** — Vous comprenez les mécanismes techniques sous-jacents
3. **Esprit critique** — Vous pouvez identifier des problèmes ou des améliorations
4. **Honnêteté intellectuelle** — Vous distinguez ce que vous comprenez de ce que vous ne comprenez pas

---

## 5. Conseils de préparation

- **Relisez votre code** avant l'entretien, en particulier les parties complexes
- **Comprenez les décorateurs NestJS** utilisés dans vos contrôleurs et services
- **Suivez le flux d'une requête** de bout en bout : Angular → NestJS → PostgreSQL → retour
- **Testez mentalement** quelques scénarios d'erreur dans votre application
- **Réfléchissez** à vos interactions avec l'IA : qu'avez-vous accepté tel quel ? Qu'avez-vous modifié ?

> **Rappel :** Dire « je ne sais pas » honnêtement vaut mieux qu'inventer une réponse. L'évaluateur valorise l'honnêteté et la démarche de réflexion.

---

## 6. Grille de correction

### Vue d'ensemble

| Critère | Points |
|---|---|
| C1 — Compréhension fonctionnelle du code | /8 |
| C2 — Maîtrise des concepts techniques | /6 |
| C3 — Esprit critique et analyse | /4 |
| C4 — Honnêteté intellectuelle et recul | /2 |
| **Total** | **/20** |

---

### C1 — Compréhension fonctionnelle du code (8 points)

*L'étudiant sait expliquer ce que fait le code, son rôle dans l'application et son fonctionnement général.*

| Niveau | Points | Description |
|---|---|---|
| **Excellent** | 7–8 | Explique clairement et avec précision chaque extrait. Fait des liens entre les différentes couches (Angular, NestJS, PostgreSQL). Anticipe les questions de suivi. |
| **Satisfaisant** | 5–6 | Explique correctement la majorité des extraits avec quelques imprécisions mineures. Comprend le rôle global du code. |
| **Partiel** | 3–4 | Comprend certaines parties mais bute sur d'autres. Explications superficielles ou incomplètes pour plusieurs extraits. |
| **Insuffisant** | 1–2 | Peine à expliquer le code. Confond les rôles ou lit le code sans le comprendre. |
| **Absent** | 0 | Ne peut expliquer aucun extrait de manière cohérente. |

---

### C2 — Maîtrise des concepts techniques (6 points)

*L'étudiant comprend les mécanismes et concepts derrière le code : injection de dépendances, ORM, observables, décorateurs, etc.*

| Niveau | Points | Description |
|---|---|---|
| **Excellent** | 6 | Explique correctement les concepts sous-jacents (ex. : pourquoi `@Injectable()`, comment fonctionne TypeORM, différence entre `Subject` et `Observable`). |
| **Satisfaisant** | 4–5 | Bonne compréhension des concepts principaux avec une ou deux lacunes mineures. |
| **Partiel** | 2–3 | Compréhension partielle. Connaît la syntaxe mais peine à expliquer le « pourquoi ». |
| **Insuffisant** | 1 | Peu de maîtrise conceptuelle. Réponses très vagues ou erronées. |
| **Absent** | 0 | Aucune compréhension des concepts techniques. |

---

### C3 — Esprit critique et analyse (4 points)

*L'étudiant peut identifier des problèmes potentiels, proposer des améliorations ou anticiper des cas limites.*

| Niveau | Points | Description |
|---|---|---|
| **Excellent** | 4 | Identifie proactivement des problèmes (sécurité, performance, cas d'erreur). Propose des améliorations concrètes et réalistes. |
| **Satisfaisant** | 3 | Repère les problèmes soulevés par l'évaluateur. Propose au moins une amélioration pertinente. |
| **Partiel** | 2 | Esprit critique limité. Répond aux questions directes sans initiative propre. |
| **Insuffisant** | 1 | Difficultés à analyser son propre code de manière critique. |
| **Absent** | 0 | Aucune capacité d'analyse critique observée. |

---

### C4 — Honnêteté intellectuelle et recul sur l'IA (2 points)

*L'étudiant est transparent sur ce qu'il comprend ou non, et démontre une réflexion sur son usage de l'IA.*

| Niveau | Points | Description |
|---|---|---|
| **Complet** | 2 | Distingue clairement ce qu'il a compris de ce qu'il ne maîtrise pas. Décrit de manière réfléchie son usage de l'IA (vérification, correction, apprentissage). |
| **Partiel** | 1 | Admet ses lacunes quand pressé, mais peu de recul spontané sur l'utilisation de l'IA. |
| **Absent** | 0 | Prétend tout comprendre sans pouvoir le démontrer, ou refuse de réfléchir à son usage de l'IA. |

---