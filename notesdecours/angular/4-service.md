# Services

Les services rassemblent des fonctionnalités qui n'ont pas d'interfaces dédiés. Principalement, on va créer un service par table dans notre base de données pour communiquer avec l'API. On pourrait aussi avoir un service qui gère les journaux d'erreurs ou un service qui gère les messages d'événements à afficher dans le logiciel. Dans ce dernier cas un service servirait à rassembler les messages et un composant servira à les afficher.

## Service simple

Pour créer un service exécutez la commande suivante:

```bash
ng g s nom-du-service
# Ou dans un dossier spécifique
ng g s services/nom-du-service
```

Par défaut, Angular crée un service avec `providedIn: 'root'`, ce qui le rend disponible dans toute l'application :

```typescript
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'  // Service singleton disponible partout
})
export class NomDuServiceService {
  constructor() {}
}
```

### Injection du service

#### Approche classique (constructeur)

```typescript
import { Component } from '@angular/core';
import { NomDuServiceService } from './services/nom-du-service.service';

@Component({
  selector: 'app-example',
  standalone: true
})
export class ExampleComponent {
  constructor(private nomDuService: NomDuServiceService) {}
}
```

#### Approche moderne avec inject() (Angular 14+, recommandée)

```typescript
import { Component, inject } from '@angular/core';
import { NomDuServiceService } from './services/nom-du-service.service';

@Component({
  selector: 'app-example',
  standalone: true
})
export class ExampleComponent {
  private nomDuService = inject(NomDuServiceService);
  
  // Ou en utilisant directement dans les méthodes
  ngOnInit() {
    this.nomDuService.getData();
  }
}
```

**Avantages de inject():**
- Plus concis et lisible
- Peut être utilisé en dehors du constructeur (dans les propriétés de classe)
- Meilleure compatibilité avec les fonctions standalone

## Marque substitutive (placeholder)

Nous n'avons pas encore de serveur, ce qui nous empêche de faire nos appels d'API. Par contre, nous pouvons préparer nos services d'appels aux API afin de limiter les modifications lorsque ce sera prêt. Nos fonctions qui consulteront l'API vont retourner le type `Observable<T>` (il faut l'importer de rxjs). Nous devons donc retourner le même type sans appel d'API, nous aurons donc besoin de la fonction `of` de rxjs qui retourne le même type. `of` nous servira de simulacre (mock) d'appel HTTP.

```typescript
import { Injectable } from '@angular/core';
import { Observable, of } from 'rxjs';

interface User {
  id: number;
  name: string;
  email: string;
}

@Injectable({
  providedIn: 'root'
})
export class UsersService {
  private users: User[] = [
    { id: 1, name: 'Alice', email: 'alice@example.com' },
    { id: 2, name: 'Bob', email: 'bob@example.com' }
  ];
  
  getAll(): Observable<User[]> {
    return of(this.users);
  }
  
  getById(id: number): Observable<User | undefined> {
    return of(this.users.find(u => u.id === id));
  }
}
```

## Utilisation dans le composant

### Approche classique avec subscribe

```typescript
import { Component, OnInit, inject } from '@angular/core';
import { UsersService } from './services/users.service';

@Component({
  selector: 'app-users',
  standalone: true,
  template: `
    <ul class="space-y-2">
      @for (user of users; track user.id) {
        <li class="p-2 border rounded">{{ user.name }} - {{ user.email }}</li>
      }
    </ul>
    @if (error) {
      <p class="text-red-600">{{ error }}</p>
    }
  `
})
export class UsersComponent implements OnInit {
  private usersService = inject(UsersService);
  
  users: User[] = [];
  error: string = '';
  
  ngOnInit(): void {
    this.usersService.getAll().subscribe({
      next: (users) => {
        this.users = users;
      },
      error: (err) => {
        this.users = [];
        this.error = err.message || 'Erreur lors du chargement';
      }
    });
  }
}
```

**Note:** La syntaxe moderne de `subscribe()` utilise un objet avec les propriétés `next`, `error`, et `complete` au lieu de fonctions en paramètres séparés.

### Approche moderne avec signals (Angular 16+, recommandée)

Bien que le signal est plus simple, il garde un observable ouvert en arrière-plan, donc l'abonnement est toujours actif. Pour un usage unique, l'approche avec `subscribe` est préférable pour éviter les fuites de mémoire.

```typescript
import { Component, OnInit, signal, inject } from '@angular/core';
import { UsersService } from './services/users.service';

@Component({
  selector: 'app-users',
  standalone: true,
  template: `
    <ul class="space-y-2">
      @for (user of users(); track user.id) {
        <li class="p-2 border rounded">{{ user.name }} - {{ user.email }}</li>
      }
    </ul>
    @if (error()) {
      <p class="text-red-600">{{ error() }}</p>
    }
  `
})
export class UsersComponent implements OnInit {
  private usersService = inject(UsersService);
  
  users = signal<User[]>([]);
  error = signal<string>('');
  
  ngOnInit(): void {
    this.usersService.getAll().subscribe({
      next: (users) => this.users.set(users),
      error: (err) => {
        this.users.set([]);
        this.error.set(err.message || 'Erreur lors du chargement');
      }
    });
  }
}
```