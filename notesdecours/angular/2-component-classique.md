# Composant - Approche Classique

Si vous créez un composant, vous allez voir 3 fichiers (4 si les tests sont activés). Chaque composant a son propre dossier. Par exemple, pour le composant login, vous trouvez dans le dossier login:

 * login.component.html - La vue, gère l'affichage seulement
 * login.component.scss - Votre feuille de style lié à la vue. Angular va modifier toutes les balises de la vue et tous les sélecteurs CSS, il ajoute un attribut HTML, par exemple, '_ngcontent-gvp-c45'.
 * login.component.ts - Le contrôleur, fait le lien entre les fichiers du composant et avec le reste du logiciel
 * login.component.spec.ts - Fichier de test unitaire

## Cycle de vie du composant

Tout ce qui est publique dans la classe du contrôleur est disponible dans la vue (propriété et fonction). Fonctions importantes du cycle de vie:

  1. **constructor**: Le constructeur est appelé à la création de la composante, on va le laisser vide presque tout le temps. En paramètre, on va lui donner les services nécessaire au fonctionnement de notre composante (ex: `constructor(private formBuilder: FormBuilder) {}`). L'injection de dépendance va donner le bon service à notre classe qui va le garder en propriété privée.
  2. **ngOnInit**: Cette fonction est appelée lorsque le gabarit (template) est généré et prêt à être manipulé. On va y inscrire le code à exécuter au démarrage du composant. Implémentez l'interface `OnInit` pour utiliser cette méthode.

## Afficher une valeur

Pour afficher une valeur dans la vue, il faut la déclarer dans le contrôleur puis utiliser les moustaches dans la vue:

```typescript
// Fichier TS - Approche classique
export class LoginComponent {
  count: number = 0;
}
```

```html
<!-- Fichier HTML -->
{{ count }}
```

## Fonction et événement

Pour utiliser une fonction, il faut lier une fonction du contrôleur à un événement DOM dans le HTML. Par exemple, on va ajouter un onClick sur un bouton qui incrémente le compte:

```typescript
// Fichier TS - Approche classique
export class LoginComponent {
  count: number = 0;
  
  incrementer(): void {
    this.count++;
  }
}
```

```html
<!-- Fichier HTML -->
{{ count }} <br/>
<button (click)="incrementer()" class="px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600">
  Incrémenter
</button>
```

Les parenthèses sont importantes autour du `click` dans le HTML, c'est ce qui indique à Angular qu'on va lier un événement.

## Manipulation d'attributs

On peut aussi manipuler les attributs HTML, par exemple si on veut ajouter une classe CSS à une balise HTML selon certaines conditions. Le cas classique est d'afficher un message d'erreur seulement lorsqu'il y en a un:

```html
<div class="p-4 bg-red-100 border border-red-400 text-red-700 rounded" 
     [ngClass]="{'hidden': error == ''}">
  {{error}}
</div>
```

`ngClass` permet d'ajouter des classes selon une condition. Il faut lui donner un objet anonyme, la 'clé' est le nom de la classe à ajouter et sa valeur est la condition. Dans cet exemple on ajoute la classe `hidden` (Tailwind) si `error` est vide.

## Conditions et boucles

On peut enlever complètement la balise lorsqu'elle n'est pas nécessaire avec l'instruction `*ngIf` (l'étoile est importante, sans elle il faudrait 'wrapper' notre balise par une balise ng-template):

```html
<div class="p-4 bg-red-100 border border-red-400 text-red-700 rounded" *ngIf="error">
  {{error}}
</div>
```

`error` est une string, une string vide vaut false. Si `error` est vide la balise ne sera pas affichée et le contenu de la balise ne sera pas interprété par Angular. Si on a plusieurs messages d'erreurs, on peut utiliser l'instruction `*ngFor`:

```html
<div class="p-4 bg-red-100 border border-red-400 text-red-700 rounded" *ngIf="errors.length > 0">
  <ul class="list-disc list-inside">
    <li *ngFor="let error of errors">
      {{error}}
    </li>
  </ul>
</div>
```

## Liaison à deux-sens (2 way binding)

C'est possible de lier une propriété du contrôleur à un champ modifiable par l'utilisateur. En plus d'afficher la valeur par défaut, l'utilisateur pourra modifier directement cette propriété:

```typescript
// Fichier TS - N'oubliez pas d'importer FormsModule
import { Component } from '@angular/core';
import { FormsModule } from '@angular/forms';

@Component({
  selector: 'app-login',
  imports: [FormsModule],
  templateUrl: './login.component.html'
})
export class LoginComponent {
  username: string = '';
}
```

```html
<!-- Fichier HTML -->
<input [(ngModel)]="username" 
       class="px-4 py-2 border border-gray-300 rounded focus:outline-none focus:ring-2 focus:ring-blue-500" 
       placeholder="Nom d'utilisateur" />
```

## Sous-composants

### Avec modules (approche classique)

Déclarez le composant dans le module:

```typescript
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';
import { LoginComponent } from './login/login.component';
import { AppComponent } from './app.component';

@NgModule({
  declarations: [LoginComponent, AppComponent],
  imports: [CommonModule],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

Parfois on va aussi vouloir transférer des données entre le composant parent et le composant enfant.

### Parent → Enfant

On commence par configurer l'enfant, on ajoute l'import `Input` de `@angular/core`. Ensuite on ajoute la propriété à recevoir:

```typescript
import { Component, Input } from '@angular/core';

@Component({
  selector: 'app-test-enfant',
  template: '{{nombre}}'
})
export class TestEnfantComponent {
  @Input() nombre: number = 0;
}
```

Maintenant on configure le parent:

```typescript
import { Component } from '@angular/core';
import { TestEnfantComponent } from './test-enfant/test-enfant.component';

@Component({
  selector: 'app-parent',
  imports: [TestEnfantComponent],
  template: '<app-test-enfant [nombre]="nombreAEnvoyer"></app-test-enfant>'
})
export class ParentComponent {
  nombreAEnvoyer: number = 42;
}
```

L'enfant va donc afficher 42. Si le parent modifie `nombreAEnvoyer`, `nombre` de l'enfant sera aussi modifié. Par contre l'inverse n'est pas vrai.

#### Réagir aux changements avec ngOnChanges

Pour effectuer une action lorsque l'input change:

```typescript
import { Component, Input, OnChanges, SimpleChanges } from '@angular/core';

export class TestEnfantComponent implements OnChanges {
  @Input() nombre: number = 0;
  
  ngOnChanges(changes: SimpleChanges): void {
    if (changes['nombre']) {
      console.log('Nombre changé:', changes['nombre'].currentValue);
    }
  }
}
```

### Enfant → Parent

Si on veut faire l'inverse, notre enfant veut envoyer des données vers le parent. Par exemple l'enfant est un formulaire de modification et on a sauvegardé, le parent doit fermer cet enfant:

```typescript
import { Component, Output, EventEmitter } from '@angular/core';

@Component({
  selector: 'app-test-enfant',
  template: `
    <button (click)="calculerTotal()" 
            class="px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600">
      Calculer
    </button>
  `
})
export class TestEnfantComponent {
  @Output() totalEvent = new EventEmitter<number>();
  
  nb1: number = 1;
  nb2: number = 2;
  
  calculerTotal(): void {
    const total = this.nb1 + this.nb2;
    this.totalEvent.emit(total);
  }
}
```

Maintenant le parent:

```typescript
import { Component } from '@angular/core';
import { TestEnfantComponent } from './test-enfant/test-enfant.component';

@Component({
  selector: 'app-parent',
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
totalEvent = new EventEmitter<{total: number, timestamp: Date}>();

calculerTotal(): void {
  this.totalEvent.emit({ total: this.nb1 + this.nb2, timestamp: new Date() });
}
```
