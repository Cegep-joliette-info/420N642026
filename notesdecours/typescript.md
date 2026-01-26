# TypeScript

TypeScript permet d'écrire du JavaScript typé. Un fichier TypeScript (fichier *.ts) doit être compilé en fichier JavaScript afin d'être compris par node ou le navigateur.

Webstorm permet de compiler les fichiers TypeScript facilement. Il y a deux méthodes:

 1. Dans le coin en bas à droite de l'IDE, cliquez sur "TypeScript 5.x" (le numéro de version peut changer). Sélectionnez Compile et choisissez les fichiers à compiler.
 2. Dans les préférences de l'IDE, dans "Languages & Frameworks" puis dans TypeScript, cochez la case "Recompile on changes".
 
Angular s'occupe de compiler les fichiers TypeScript, donc une étape de moins pour nous!

## Configuration

*Section mise à jour par LLM*

Il faut un fichier "tsconfig.json" à la racine de votre projet, il va indiquer au compilateur comment transformer le TypeScript en JavaScript. Des fichiers de bases sont données sur le [GitHub de tsconfig](https://github.com/tsconfig/bases/tree/master/bases). Nous allons utiliser le fichier nodexx.json (remplacez les "x" par la dernière version LTS de Node.js) pour notre serveur qui s'exécutera via node. Par contre, [Angular donne sa propre recommendation de tsconfig.json](https://angular.io/guide/typescript-configuration) que nous allons utiliser pour le client.

### Configuration Angular (v17+)

Voici un exemple de tsconfig.json initial pour un projet Angular:

```json
{
  "compileOnSave": false,
  "compilerOptions": {
    "baseUrl": "./",
    "forceConsistentCasingInFileNames": true,
    "strict": true,
    "noImplicitOverride": true,
    "noPropertyAccessFromIndexSignature": true,
    "noImplicitReturns": true,
    "noFallthroughCasesInSwitch": true,
    "sourceMap": true,
    "declaration": false,
    "downlevelIteration": true,
    "experimentalDecorators": true,
    "moduleResolution": "node",
    "importHelpers": true,
    "target": "ES2022",
    "module": "ES2022",
    "useDefineForClassFields": false,
    "lib": [
      "ES2022",
      "dom"
    ]
  },
  "angularCompilerOptions": {
    "enableI18nLegacyMessageIdFormat": false,
    "strictInjectionParameters": true,
    "strictInputAccessModifiers": true,
    "strictTemplates": true
  }
}
```

### Explication des options principales

**Options de compilation TypeScript:**
 * [target](https://www.typescriptlang.org/tsconfig#target): Version de EcmaScript (JavaScript) après la compilation. Angular recommande **ES2022** (janvier 2026). Le navigateur doit supporter cette version - le contrôle de compatibilité se fait via [Browserslist](https://github.com/browserslist/browserslist).
 * [module](https://www.typescriptlang.org/tsconfig#module): Comment TypeScript gère les modules. Angular utilise **ES2022**, qui supporte les import/export ES6 que vous avez vus dans vos cours précédents.
 * [lib](https://www.typescriptlang.org/tsconfig#lib): Les librairies TypeScript utilisées pour la validation du code. Angular utilise `["ES2022", "dom"]` pour permettre l'utilisation des fonctionnalités ES2022 et la manipulation du DOM HTML.
 * [strict](https://www.typescriptlang.org/tsconfig#strict): Active la validation stricte du typage. **Toujours à true** - assure que les variables sont correctement déclarées et typées.
 * [sourceMap](https://www.typescriptlang.org/tsconfig#sourceMap): Génère des fichiers .map pour lier le JS compilé au TS source, essentiel pour le débogage.
 * [declaration](https://www.typescriptlang.org/tsconfig#declaration): Génère des fichiers .d.ts avec les définitions de types. À false pour Angular - vous devez tout typer explicitement!

**Options spécifiques à Angular:**
 * [experimentalDecorators](https://www.typescriptlang.org/tsconfig#experimentalDecorators): Active les décorateurs (@Component, @Injectable, etc.) essentiels à Angular.
 * [moduleResolution](https://www.typescriptlang.org/tsconfig#moduleResolution): "node" - détermine comment TypeScript résout les imports de modules.
 * [importHelpers](https://www.typescriptlang.org/tsconfig#importHelpers): À true, utilise la librairie "tslib" pour les fonctions utilitaires de compilation au lieu de les dupliquer dans chaque fichier.
 * [downlevelIteration](https://www.typescriptlang.org/tsconfig#downlevelIteration): Permet une gestion correcte des itérateurs pour les cibles ES5/ES3 (hérité, moins crucial en 2026).
 * [useDefineForClassFields](https://www.typescriptlang.org/tsconfig#useDefineForClassFields): À false pour Angular - assure la compatibilité avec le comportement des champs de classe Angular.

**Options strictes supplémentaires (Angular v17+):**
 * [noImplicitOverride](https://www.typescriptlang.org/tsconfig#noImplicitOverride): Exige le mot-clé `override` lors de la surcharge de méthodes.
 * [noPropertyAccessFromIndexSignature](https://www.typescriptlang.org/tsconfig#noPropertyAccessFromIndexSignature): Force l'utilisation de la notation `[]` pour accéder aux propriétés indexées.
 * [noImplicitReturns](https://www.typescriptlang.org/tsconfig#noImplicitReturns): Vérifie que toutes les branches d'une fonction retournent une valeur.
 * [noFallthroughCasesInSwitch](https://www.typescriptlang.org/tsconfig#noFallthroughCasesInSwitch): Empêche les cas switch sans break/return.

## Utilisation

TS est entièrement compatible à JS (selon votre configuration tsconfig.json).

Pour typer un élément en TS, un met un ':' suivit du type immédiatement après le nom de l'élément. Par exemple:

```typescript
let count : number = 0;
```

Les types [disponibles](https://www.typescriptlang.org/docs/handbook/basic-types.html):
 * boolean
 * number, autant pour les int que pour les floats
 * bigint, pour les nombres entiers supérieurs à 2
 * string, vous pouvez utiliser ', " ou ` comme en JS
 * [] après un type de base pour en faire un tableau, exemple: number[]
 * unknown, peut être n'importe type, mais ne permet pas les opérations typés
 * any, peut-être n'importe type, c'est comme une vrai variable JS
 * void, n'a aucune valeur, peut seulement être null, utile pour les valeur de retour
 * null, peut seulement être null
 * undefined, peut seulement être undefined
 * never, sur une valeur de retour, signifie que la fonction ne termine pas normalement, par exemple avec un throw

Autres types:

 * Tuple: Permet de mettre plusieurs types dans un tableau: [string, number]
 * Enum: JS ne permet pas les enums, TS ajoute cette fonctionnalité, voir la doc

On peut aussi combiner des valeurs avec le |, par exemple une variable qui peut être un entier ou null:

```typescript
let id : number | null = null;
```

### Variables

On va toujours déclarer les variables avec le mot-clé let, n'utilisez pas le mot-clé var.

### Classes

Par défaut, les propriétés de classes sont publiques, vous pouvez utilisez les mots-clés public, private et protected.

Vous avez aussi accès aux mots-clés abstract, static, extends, implements, constructor, get, set etc.

### Point-virgule

En JS tout comme en TS, le point-virgule est optionnel. Par contre, sans point virgule c'est JavaScript ou le compilateur qui décide où termine la ligne de code...

De plus, si vous avez une ligne qui débute par (, [ ou `, vous DEVEZ mettre un point-virgule dans la ligne d'avant. Donc tant qu'à mettre des point-virgules "une fois de temps en temps" et d'avoir un code non-uniforme, gardez vos pratique PHP, C# et C++ et utilisez les points-virgules.