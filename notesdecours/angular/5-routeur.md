# Routeur

Le routeur Angular permet de naviguer entre différentes vues/composants en fonction de l'URL. Avec les composants standalone et Angular moderne, la configuration du routeur est simplifiée.

## Configuration de base

### 1. Fichier app.routes.ts

Créez un fichier `app.routes.ts` à la racine de votre application pour définir vos routes:

```typescript
import { Routes } from '@angular/router';

export const routes: Routes = [
  { path: '', component: HomeComponent },
  { path: 'users', component: UsersListComponent },
  { path: 'users/:id', component: UserViewComponent },
  { path: '**', redirectTo: '' }  // Route 404
];
```

**Points importants sur l'ordre des routes:**

L'ordre des routes est crucial. Le routeur prend la première route qui correspond à l'URL. Exemple incorrect:

```typescript
// INCORRECT - 'users/new' matchera 'users/:id' avec id='new'
{ path: 'users/:id', component: UserEditComponent },
{ path: 'users/new', component: UserNewComponent }

// CORRECT - Les routes spécifiques avant les routes paramétrées
{ path: 'users/new', component: UserNewComponent },
{ path: 'users/:id', component: UserEditComponent }
```

### 2. Configuration dans app.config.ts

Fournissez le routeur dans la configuration de l'application:

```typescript
import { ApplicationConfig } from '@angular/core';
import { provideRouter, withComponentInputBinding } from '@angular/router';
import { routes } from './app.routes';

export const appConfig: ApplicationConfig = {
  providers: [
    provideRouter(
      routes,
      withComponentInputBinding()  // Permet l'utilisation d'input() pour les paramètres
    )
  ]
};
```

**Option `withComponentInputBinding()`:** Cette option permet de recevoir les paramètres de route directement comme inputs du composant (approche moderne recommandée).

### 3. Template principal (app.component.html)

Ajoutez `<router-outlet>` dans votre template principal:

```typescript
import { Component } from '@angular/core';
import { RouterOutlet } from '@angular/router';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [RouterOutlet],
  template: `
    <header class="bg-blue-600 text-white p-4">
      <h1>Mon Application</h1>
    </header>
    <main class="container mx-auto p-4">
      <router-outlet />
    </main>
  `
})
export class AppComponent {}
```

## Récupérer les paramètres de route

### Approche moderne avec input() (recommandée)

Avec `withComponentInputBinding()`, les paramètres de route deviennent des inputs:

```typescript
import { Component, input, effect } from '@angular/core';

@Component({
  selector: 'app-user-view',
  standalone: true,
  template: `
    <div class="p-4">
      <h2>Utilisateur #{{ id() }}</h2>
    </div>
  `
})
export class UserViewComponent {
  // Le paramètre 'id' de la route devient un input
  id = input.required<string>();
  
  constructor() {
    effect(() => {
      console.log('User ID:', this.id());
      // Charger les données de l'utilisateur
    });
  }
}
```

### Approche classique avec ActivatedRoute

```typescript
import { Component, OnInit, inject } from '@angular/core';
import { ActivatedRoute } from '@angular/router';

@Component({
  selector: 'app-user-view',
  standalone: true,
  template: `<div>Utilisateur #{{ userId }}</div>`
})
export class UserViewComponent implements OnInit {
  private route = inject(ActivatedRoute);
  userId: string = '';
  
  ngOnInit(): void {
    // Snapshot (valeur immédiate)
    this.userId = this.route.snapshot.params['id'];
    
    // Ou observable (pour les changements de paramètres)
    this.route.params.subscribe(params => {
      this.userId = params['id'];
    });
  }
}
```

## Navigation programmatique

### Avec Router

```typescript
import { Component, inject } from '@angular/core';
import { Router } from '@angular/router';

@Component({
  selector: 'app-user-form',
  standalone: true,
  template: `
    <button (click)="goToList()" class="px-4 py-2 bg-blue-500 text-white rounded">
      Retour à la liste
    </button>
  `
})
export class UserFormComponent {
  private router = inject(Router);
  
  goToList(): void {
    this.router.navigate(['/users']);
  }
  
  goToUser(id: number): void {
    this.router.navigate(['/users', id]);
    // Équivalent à: this.router.navigate([`/users/${id}`]);
  }
  
  // Avec query params
  searchUsers(query: string): void {
    this.router.navigate(['/users'], { 
      queryParams: { search: query, page: 1 } 
    });
    // URL résultante: /users?search=john&page=1
  }
}
```

### Liens avec RouterLink

```typescript
import { Component } from '@angular/core';
import { RouterLink, RouterLinkActive } from '@angular/router';

@Component({
  selector: 'app-nav',
  standalone: true,
  imports: [RouterLink, RouterLinkActive],
  template: `
    <nav class="flex gap-4 p-4 bg-gray-800">
      <a routerLink="/" 
         routerLinkActive="text-blue-400"
         [routerLinkActiveOptions]="{exact: true}"
         class="text-white hover:text-blue-300">
        Accueil
      </a>
      <a routerLink="/users" 
         routerLinkActive="text-blue-400"
         class="text-white hover:text-blue-300">
        Utilisateurs
      </a>
      <a [routerLink]="['/users', userId]"
         class="text-white hover:text-blue-300">
        Mon profil
      </a>
    </nav>
  `
})
export class NavComponent {
  userId = 123;
}
```

## Lazy Loading (chargement différé)

Pour optimiser les performances, chargez les modules seulement quand nécessaire:

### Structure des fichiers

```
src/app/
  ├── app.routes.ts
  ├── users/
  │   ├── user.routes.ts
  │   ├── list/
  │   │   └── users-list.component.ts
  │   └── view/
  │       └── user-view.component.ts
```

### app.routes.ts (routes principales)

```typescript
import { Routes } from '@angular/router';

export const routes: Routes = [
  { path: '', component: HomeComponent },
  {
    path: 'users',
    loadChildren: () => import('./users/user.routes')
  },
  { path: '**', redirectTo: '' }
];
```

### users/user.routes.ts (routes du feature)

```typescript
import { Routes } from '@angular/router';

export const routes: Routes = [
  {
    path: '',
    loadComponent: () => import('./list/users-list.component').then(m => m.UsersListComponent)
  },
  {
    path: ':id',
    loadComponent: () => import('./view/user-view.component').then(m => m.UserViewComponent)
  }
];

export default routes;
```

Les routes résultantes:
- `/users` → UsersListComponent (chargé à la demande)
- `/users/:id` → UserViewComponent (chargé à la demande)

## Routes imbriquées (Nested Routes)

Pour créer des layouts avec sous-routes:

```typescript
import { Routes } from '@angular/router';

export const routes: Routes = [
  {
    path: 'admin',
    component: AdminLayoutComponent,  // Layout parent avec <router-outlet>
    children: [
      { path: '', component: AdminDashboardComponent },
      { path: 'users', component: AdminUsersComponent },
      { path: 'settings', component: AdminSettingsComponent }
    ]
  }
];
```

**AdminLayoutComponent:**

```typescript
import { Component } from '@angular/core';
import { RouterOutlet, RouterLink } from '@angular/router';

@Component({
  selector: 'app-admin-layout',
  standalone: true,
  imports: [RouterOutlet, RouterLink],
  template: `
    <div class="flex h-screen">
      <aside class="w-64 bg-gray-800 text-white p-4">
        <nav class="flex flex-col gap-2">
          <a routerLink="/admin" [routerLinkActiveOptions]="{exact: true}" 
             routerLinkActive="bg-blue-600" class="p-2 rounded">
            Dashboard
          </a>
          <a routerLink="/admin/users" routerLinkActive="bg-blue-600" 
             class="p-2 rounded">
            Utilisateurs
          </a>
          <a routerLink="/admin/settings" routerLinkActive="bg-blue-600" 
             class="p-2 rounded">
            Paramètres
          </a>
        </nav>
      </aside>
      
      <main class="flex-1 p-6 overflow-auto">
        <router-outlet />
      </main>
    </div>
  `
})
export class AdminLayoutComponent {}
```

Les routes résultantes:
- `/admin` → AdminLayoutComponent + AdminDashboardComponent
- `/admin/users` → AdminLayoutComponent + AdminUsersComponent
- `/admin/settings` → AdminLayoutComponent + AdminSettingsComponent

## Query Params et Fragments

### Lire les query params

```typescript
import { Component, OnInit, inject } from '@angular/core';
import { ActivatedRoute } from '@angular/router';

@Component({
  selector: 'app-users-list',
  standalone: true,
  template: `<div>Page: {{ page }}, Recherche: {{ search }}</div>`
})
export class UsersListComponent implements OnInit {
  private route = inject(ActivatedRoute);
  page = 1;
  search = '';
  
  ngOnInit(): void {
    this.route.queryParams.subscribe(params => {
      this.page = params['page'] || 1;
      this.search = params['search'] || '';
    });
  }
}
```

### Naviguer avec query params

```typescript
// URL: /users?search=john&page=2
this.router.navigate(['/users'], {
  queryParams: { search: 'john', page: 2 }
});

// Avec fragment (anchor)
// URL: /users#section-top
this.router.navigate(['/users'], {
  fragment: 'section-top'
});
```

**Documentation officielle:** https://angular.dev/guide/routing