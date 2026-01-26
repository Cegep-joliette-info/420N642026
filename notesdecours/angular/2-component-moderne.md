# Composant - Approche Moderne (Angular 14+)

Si vous créez un composant, vous allez voir 3 fichiers (4 si les tests sont activés). Chaque composant a son propre dossier. Par exemple, pour le composant login, vous trouvez dans le dossier login:

 * login.component.html - La vue, gère l'affichage seulement
 * login.component.scss - Votre feuille de style lié à la vue. Angular va modifier toutes les balises de la vue et tous les sélecteurs CSS, il ajoute un attribut HTML, par exemple, '_ngcontent-gvp-c45'.
 * login.component.ts - Le contrôleur, fait le lien entre les fichiers du composant et avec le reste du logiciel
 * login.component.spec.ts - Fichier de test unitaire

## Composants Standalone

Depuis Angular 14+, les composants standalone sont l'approche recommandée. Ils ne nécessitent plus de NgModule et simplifient la structure. Un composant standalone ressemble à:

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-login',
  imports: [],
  templateUrl: './login.html',
  styleUrl: './login.scss'
})
export class Login {
  // Votre code ici
}
```

## Cycle de vie du composant

Tout ce qui est publique dans la classe du contrôleur est disponible dans la vue (propriété et fonction). Fonctions importantes du cycle de vie:

  1. **constructor**: Le constructeur est appelé à la création de la composante, on va le laisser vide presque tout le temps. En paramètre, on va lui donner les services nécessaire au fonctionnement de notre composante (ex: `constructor(private formBuilder: FormBuilder) {}`). L'injection de dépendance va donner le bon service à notre classe qui va le garder en propriété privée. Depuis Angular 14+, vous pouvez aussi utiliser la fonction `inject()` directement dans les propriétés.
  2. **ngOnInit**: Cette fonction est appelée lorsque le gabarit (template) est généré et prêt à être manipulé. On va y inscrire le code à exécuter au démarrage du composant. Implémentez l'interface `OnInit` pour utiliser cette méthode.

### Exemple avec inject() et effect()

Depuis Angular 14+, la fonction `inject()` permet d'injecter des services directement dans les propriétés de classe, sans passer par le constructeur. C'est particulièrement utile avec les composants standalone:

```typescript
import { Component, signal, effect, inject } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Router } from '@angular/router';

@Component({
  selector: 'app-user-profile',
  standalone: true,
  template: `
    <div class="p-4">
      <h2>{{ username() }}</h2>
      <p>Connexions: {{ loginCount() }}</p>
    </div>
  `
})
export class UserProfileComponent {
  // Injection de services avec inject()
  private http = inject(HttpClient);
  private router = inject(Router);
  
  // Signals pour l'état réactif
  username = signal('Invité');
  loginCount = signal(0);
  
  constructor() {
    // effect() permet de réagir aux changements de signals
    effect(() => {
      console.log(`L'utilisateur ${this.username()} s'est connecté ${this.loginCount()} fois`);
      
      // Si le compteur atteint 10, rediriger vers une page de récompense
      if (this.loginCount() >= 10) {
        this.router.navigate(['/rewards']);
      }
    });
    
    // Autre effect pour sauvegarder automatiquement
    effect(() => {
      // Sauvegarde automatique quand username change
      if (this.username() !== 'Invité') {
        localStorage.setItem('lastUser', this.username());
      }
    });
  }
}
```

**Points importants sur effect():**
- `effect()` s'exécute automatiquement quand un signal qu'il lit change
- Doit être appelé dans un contexte d'injection (constructor, champ de classe)
- Utile pour les effets de bord (logging, navigation, sauvegarde, etc.)
- Évitez de modifier des signals dans un effect (préférez `computed()` pour les valeurs dérivées)

## Afficher une valeur

### Signals (Angular 16+, recommandé)

Angular introduit les signals pour une gestion d'état plus réactive et performante:

```typescript
import { Component, signal } from '@angular/core';

export class LoginComponent {
  count = signal(0);
}
```

```html
{{ count() }}
```

Notez les parenthèses `count()` pour lire un signal dans le template.

## Fonction et événement

### Avec Signals (recommandé)

```typescript
import { Component, signal } from '@angular/core';

export class LoginComponent {
  count = signal(0);
  
  incrementer(): void {
    this.count.update(value => value + 1);
    // Ou: this.count.set(this.count() + 1);
  }
}
```

```html
{{ count() }} <br/>
<button (click)="incrementer()" class="px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600">
  Incrémenter
</button>
```

Les parenthèses sont importantes autour du `click` dans le HTML, c'est ce qui indique à Angular qu'on va lier un événement.

## Manipulation d'attributs

On peut aussi manipuler les attributs HTML, par exemple si on veut ajouter une classe CSS à une balise HTML selon certaines conditions.

### Syntaxe moderne avec @if (Angular 17+, recommandée)

Angular 17 introduit une nouvelle syntaxe de contrôle de flux plus intuitive:

```html
@if (error) {
  <div class="p-4 bg-red-100 border border-red-400 text-red-700 rounded">
    {{error}}
  </div>
}
```

## Conditions et boucles

### Nouvelle syntaxe de contrôle (Angular 17+, recommandée)

Angular 17 introduit `@if`, `@for`, `@switch` qui sont plus lisibles et performants:

```html
@if (error) {
  <div class="p-4 bg-red-100 border border-red-400 text-red-700 rounded">
    {{error}}
  </div>
}

@if (errors.length > 0) {
  <div class="p-4 bg-red-100 border border-red-400 text-red-700 rounded">
    <ul class="list-disc list-inside">
      @for (error of errors; track error) {
        <li>{{error}}</li>
      }
    </ul>
  </div>
}
```

Notez que `@for` requiert un `track` pour optimiser le rendu (généralement `track $index` ou une propriété unique).

#### @else et @empty

```html
@if (user) {
  <p>Bienvenue {{user.name}}</p>
} @else {
  <p>Veuillez vous connecter</p>
}

@for (item of items; track item.id) {
  <div>{{item.name}}</div>
} @empty {
  <p>Aucun élément à afficher</p>
}
```

Pour plus de détails: https://angular.dev/guide/templates/control-flow

## Liaison à deux-sens (2 way binding)

### Avec Signals (Angular 16+)

Pour les signals, utilisez le two-way binding avec des models:

```typescript
import { Component, model } from '@angular/core';

export class LoginComponent {
  username = model('');
}
```

```html
<input [(ngModel)]="username" 
       class="px-4 py-2 border border-gray-300 rounded focus:outline-none focus:ring-2 focus:ring-blue-500" />
```

## Sous-composants

Vous pouvez inclure des composants dans d'autres composants. Vous pouvez déjà le voir dans la vue du composant racine.

### Avec composants standalone

Importez le composant enfant dans le parent:

```typescript
import { Component } from '@angular/core';
import { LoginComponent } from './login/login.component';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [LoginComponent],
  template: '<app-login></app-login>'
})
export class AppComponent {}
```

Parfois on va aussi vouloir transférer des données entre le composant parent et le composant enfant.

### Parent → Enfant

#### Nouvelle approche avec input() (Angular 16+, recommandée)

```typescript
import { Component, input } from '@angular/core';

@Component({
  selector: 'app-test-enfant',
  standalone: true,
  template: '{{nombre()}}'
})
export class TestEnfantComponent {
  nombre = input<number>(0); // 0 est la valeur par défaut
  // Ou avec required: nombre = input.required<number>();
}
```

#### Réagir aux changements

Pour effectuer une action lorsque l'input change, utilisez `effect()` avec les signals:

```typescript
import { Component, input, effect } from '@angular/core';

export class TestEnfantComponent {
  nombre = input<number>(0);
  
  constructor() {
    effect(() => {
      console.log('Nombre changé:', this.nombre());
    });
  }
}
```

### Enfant → Parent

#### Nouvelle approche avec output() (Angular 17.3+, recommandée)

```typescript
import { Component, output } from '@angular/core';

@Component({
  selector: 'app-test-enfant',
  standalone: true,
  template: `
    <button (click)="calculerTotal()" 
            class="px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600">
      Calculer
    </button>
  `
})
export class TestEnfantComponent {
  totalEvent = output<number>();
  
  nb1: number = 1;
  nb2: number = 2;
  
  calculerTotal(): void {
    const total = this.nb1 + this.nb2;
    this.totalEvent.emit(total);
  }
}
```

Le parent utilise exactement la même syntaxe:

```typescript
import { Component } from '@angular/core';
import { TestEnfantComponent } from './test-enfant/test-enfant.component';

@Component({
  selector: 'app-parent',
  standalone: true,
  imports: [TestEnfantComponent],
  template: '<app-test-enfant (totalEvent)="addTotal($event)"></app-test-enfant>'
})
export class ParentComponent {
  totaux: number[] = [];
  
  addTotal(nouveauTotal: number): void {
    this.totaux.push(nouveauTotal);
  }
}
```

Si vous voulez passer plusieurs paramètres avec Output, envoyez un objet:

```typescript
totalEvent = output<{total: number, timestamp: Date}>();

calculerTotal(): void {
  this.totalEvent.emit({ total: this.nb1 + this.nb2, timestamp: new Date() });
}
```
