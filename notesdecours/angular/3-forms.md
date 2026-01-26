# Formulaire

Avec la liaison à deux sens du dernier chapitre on peut facilement créer un formulaire simple. Angular gère bien les vérifications HTML comme required et min/max. Par contre, cette pratique est une vielle technique qui provient de feu AngularJS appelé Formulaire mené par modèle (Template driven form). La nouvelle technique, Formulaire réactif (Reactive Forms), est plus complexe, mais permet de créer des formulaires par programmation, de bien tester notre composante et de mettre plus de logique vers le fichier TS.

![Angular forms](../../images/angular-forms.png)

Source de l'image: https://stackoverflow.com/a/51435178

Pour commencer, il faut importer le module de forms:

```ts
import {FormBuilder, FormGroup, Validators} from "@angular/forms";
```

Ensuite dans notre classe il faut déclarer notre formulaire:

```ts
loginForm: FormGroup;
```

Et il faut recevoir le service de formulaires réactif dans notre classe, on modifie donc le constructeur afin de bénificier de l'injection de dépendence:

```
constructor(private formBuilder: FormBuilder) {}
```

Maintenant, dans le ngOnInit on peut initialiser notre formulaire, par exemple:

```ts
ngOnInit(): void {
  this.loginForm = this.formBuilder.group({
    username: ['', Validators.required],
    password: ['', Validators.required]
  });
}
```

Notre formulaire contient donc 2 champs: username et password. Par défaut, les deux ont une valeur vide (première valeur dans le tableau) et les deux sont requis. On peut mettre plusieurs validateurs en donnant un tableau de validateurs en 2e élément de tableau, on peut même créer nos validateurs.

Notre formulaire est prêt à être utilisé, il faut maintenant aller le lier au HTML (j'ai enlevé beaucoup de code pour garder l'essentiel):

```html
<form [formGroup]="loginForm" (ngSubmit)="onSubmit()">
  <input formControlName="username"/>
  <input formControlName="password" type="password"/>
  <button type="submit">Connexion</button>
</form>
```

Les points importants:

  1. Dans le formulaire on identifie le [formGroup] qui contient le nom de notre propriété du même type
  2. Pour chaque contrôle, on ajoute le 'formControlName' qui correspond au champ présent dans notre formGroup
  3. Petit bonus, j'ai ajouté le code nécessaire à la soumission d'un formulaire. On aurais aussi pu mettre un onClic sur le bouton

Maintenant quelques lignes de codes intéressants pour le onSubmit:

```
this.loginForm.invalid; // Booléen, vrai s'il y a une erreur
this.loginForm.getRawValue(); // Retourne un objet anonyme contenant les valeurs des champs
this.loginForm.controls.username.errors.required; // Retourne vrai si username ne respecte pas la validation required
```

La première ligne de code est utile pour enregistrer seulement s'il n'y a pas d'erreur.
La deuxième ligne de code permet de récupérer vos données. Si vous avez un modèle avec exactement les mêmes propriétés que votre formulaire, vous pouvez typer la variable qui recevra les valeurs.
La troisième ligne de code peut être utilisé dans le composant ou dans la vue. Dans le composant on pourra récupérer une seule erreur et afficher le message correspondant tandis que dans la vue on pourrait afficher tous les messages possibles avec des ngIf.

## Forms with Signals (Expérimental)

**⚠️ Fonctionnalité expérimentale** - Signal Forms sont une API expérimentale. L'API peut changer dans les versions futures. Évitez d'utiliser les APIs expérimentales dans des applications de production sans comprendre les risques.

Signal Forms gèrent l'état des formulaires en utilisant les signals Angular pour fournir une synchronisation automatique entre votre modèle de données et l'interface utilisateur.

### Les 5 étapes pour créer un formulaire

#### 1. Créer un modèle de formulaire avec signal()

Chaque formulaire commence par créer un signal qui contient le modèle de données:

```typescript
import { Component, signal } from '@angular/core';
import { form, FormField } from '@angular/forms/signals';

interface LoginData {
  email: string;
  password: string;
}

@Component({
  selector: 'app-login',
  standalone: true,
  imports: [FormField],
  templateUrl: './login.component.html'
})
export class LoginComponent {
  loginModel = signal<LoginData>({
    email: '',
    password: ''
  });
}
```

#### 2. Passer le modèle à form() pour créer un FieldTree

Ensuite, passez votre modèle de formulaire à la fonction `form()` pour créer un arbre de champs (field tree):

```typescript
loginForm = form(this.loginModel);

// Accès aux champs directement par nom de propriété
// this.loginForm.email
// this.loginForm.password
```

#### 3. Lier les inputs HTML avec la directive [formField]

Liez vos inputs HTML au formulaire en utilisant la directive `[formField]`:

```html
<form (ngSubmit)="onSubmit($event)">
  <div class="space-y-4">
    <div>
      <label for="email" class="block text-sm font-medium text-gray-700">Email</label>
      <input 
        type="email" 
        [formField]="loginForm.email"
        class="mt-1 block w-full px-3 py-2 border border-gray-300 rounded-md"
      />
    </div>
    
    <div>
      <label for="password" class="block text-sm font-medium text-gray-700">Mot de passe</label>
      <input 
        type="password" 
        [formField]="loginForm.password"
        class="mt-1 block w-full px-3 py-2 border border-gray-300 rounded-md"
      />
    </div>
    
    <button type="submit" class="w-full px-4 py-2 bg-blue-500 text-white rounded-md">
      Se connecter
    </button>
  </div>
</form>
```

Les changements de l'utilisateur (comme la saisie dans le champ) mettent automatiquement à jour le formulaire.

#### 4. Lire les valeurs avec value()

Vous pouvez accéder à l'état du champ en appelant le champ comme une fonction. Cela retourne un objet `FieldState` contenant des signals réactifs:

```typescript
// Retourne FieldState avec value(), valid(), touched(), etc.
loginForm.email(); 

// Lire la valeur actuelle
const currentEmail = loginForm.email().value();
```

Dans le template:

```html
<!-- Affiche la valeur qui se met à jour automatiquement -->>
<p>Email: {{ loginForm.email().value() }}</p>
```

#### 5. Mettre à jour les valeurs avec set()

Vous pouvez mettre à jour la valeur d'un champ programmatiquement avec `value.set()`:

```typescript
// Mettre à jour la valeur programmatiquement
loginForm.email().value.set('alice@wonderland.com');

// Le signal du modèle est aussi mis à jour
console.log(loginModel().email); // 'alice@wonderland.com'
```

### Validation

Pour ajouter de la validation, passez une fonction de schéma comme second argument à `form()`:

```typescript
import { form, FormField, required, email, minLength } from '@angular/forms/signals';

loginForm = form(this.loginModel, (schemaPath) => {
  required(schemaPath.email, { message: 'Email est requis' });
  email(schemaPath.email, { message: 'Entrez une adresse email valide' });
  required(schemaPath.password, { message: 'Mot de passe requis' });
  minLength(schemaPath.password, 8, { message: 'Minimum 8 caractères' });
});
```

### Signals d'état de champ

Chaque `field()` fournit ces signals d'état:

| Signal | Description |
|--------|-------------|
| `value()` | La valeur actuelle du champ |
| `valid()` | Retourne true si le champ passe toutes les règles de validation |
| `touched()` | Retourne true si l'utilisateur a donné le focus puis l'a retiré |
| `dirty()` | Retourne true si l'utilisateur a modifié la valeur |
| `disabled()` | Retourne true si le champ est désactivé |
| `readonly()` | Retourne true si le champ est en lecture seule |
| `pending()` | Retourne true si une validation asynchrone est en cours |
| `errors()` | Retourne un tableau d'erreurs de validation avec kind et message |

### Afficher les erreurs de validation

```html
<input type="email" [formField]="loginForm.email" />

@if (loginForm.email().touched() && !loginForm.email().valid()) {
  <div class="mt-1 text-sm text-red-600">
    @for (error of loginForm.email().errors(); track error.kind) {
      <p>{{ error.message }}</p>
    }
  </div>
}
```

### Exemple complet

```typescript
import { Component, signal } from '@angular/core';
import { form, FormField, required, email } from '@angular/forms/signals';

interface LoginData {
  email: string;
  password: string;
}

@Component({
  selector: 'app-login',
  standalone: true,
  imports: [FormField],
  template: `
    <form (ngSubmit)="onSubmit($event)" class="max-w-md mx-auto p-6 space-y-4">
      <div>
        <label class="block text-sm font-medium text-gray-700">Email</label>
        <input 
          type="email" 
          [formField]="loginForm.email"
          class="mt-1 block w-full px-3 py-2 border rounded-md"
        />
        @if (loginForm.email().touched() && !loginForm.email().valid()) {
          <p class="mt-1 text-sm text-red-600">
            {{ loginForm.email().errors()[0]?.message }}
          </p>
        }
      </div>
      
      <div>
        <label class="block text-sm font-medium text-gray-700">Mot de passe</label>
        <input 
          type="password" 
          [formField]="loginForm.password"
          class="mt-1 block w-full px-3 py-2 border rounded-md"
        />
      </div>
      
      <button 
        type="submit" 
        class="w-full px-4 py-2 bg-blue-500 text-white rounded-md hover:bg-blue-600"
      >
        Se connecter
      </button>
      
      <p class="text-sm">Email saisi: {{ loginForm.email().value() }}</p>
    </form>
  `
})
export class LoginComponent {
  loginModel = signal<LoginData>({
    email: '',
    password: ''
  });
  
  loginForm = form(this.loginModel, (schemaPath) => {
    required(schemaPath.email, { message: 'Email est requis' });
    email(schemaPath.email, { message: 'Entrez une adresse email valide' });
    required(schemaPath.password, { message: 'Mot de passe requis' });
  });
  
  onSubmit(event: Event): void {
    event.preventDefault();
    const credentials = this.loginModel();
    console.log('Connexion avec:', credentials);
  }
}
```

**Documentation complète:** https://angular.dev/essentials/signal-forms

## Bootstrap
Si vous utilisez Bootstrap, vous devez faire quelques modifications pour que la validation fonctionne bien. Commencez par enlever la référence vers le bootstrap.css du fichier angular.json. Ensuite dans le fichier src/styles.scss (qui sera inclus dans tous les modules/composants), inscrivez ceci:

```
@import "~bootstrap/scss/bootstrap";
  .ng-touched.ng-invalid {
  @extend .is-invalid
}
.ng-touched.ng-valid {
  @extend .is-valid
}
```

Ça active le style d'erreurs Bootstrap avec les classes de Angular. Exemple de HTML du formulaire:

```
<form [formGroup]="loginForm" (ngSubmit)="login()">
  <div>
    <label for="username">Nom d'utilisateur</label>
    <input formControlName="username" id="username" class="form-control"/>
    <div class="invalid-feedback">Le nom d'utilisateur est obligatoire</div>
  </div>
  <button type="submit" class="btn btn-primary">Se connecter</button>
</form>
```

Notez que la classe "invalid-feedback' apparait seulement si l'élément précédent est invalide. La fonction 'login()':

```
login():void {
  if (this.loginForm.valid) {
    // Appeler service
  }
  else {
    Object.keys(this.loginForm.controls).forEach(field => {
    const control = this.loginForm.get(field);
    control.markAsTouched({onlySelf: true});
  });
}
```
Le else est là afin de valider manuellement les champs. Sans ça, si un utilisateur clique sur le bouton sans donner le focus aux contrôles, les messages d'erreurs ne s'affichent pas.
