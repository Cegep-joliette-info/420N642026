# Communication Angular <-> Express

Maintenant que tout est placé, comment Angular peut appeler nos routes Express?

Dans notre module, il faut ajouter le module de client HTTP:

```ts
import { HttpClientModule } from "@angular/common/http";
// et ajouter HttpClientModule dans le tableau d'imports
```

Dans notre service, on va ajouter l'import suivant:

```ts
import { HttpClient } from "@angular/common/http";
```

HttpClient est un service, il faut donc l'ajouter à notre service avec l'injection de dépendance:

```ts
constructor(private http: HttpClient) {}
```

Dans notre service, on retournait 'of(tableauDobjets)'. Maintenant on va changer cette ligne par le très simple:

```ts
return this.http.get(`${this.baseUrl}/users`);
```

Cette fonction va retourner un observable, donc vos components devraient déjà fonctionner. Le baseUrl provient de où? On va l'ajouter à notre fichier environment.ts (on écrit l'url de express, soit 'http://localhost:3000'). Ensuite vous pouvez y accéder dans votre service avec 'environnement.baseUrl'.

Si votre fichier d'environnement Angular est absent, générez le avec la commande:

```ts
ng g environments
```

Pour créer un utilisateur, nous allons donc utiliser:

```ts
return this.http.post(`${this.baseUrl}/users`, newUser);
```

Il faut donc passer l'objet à envoyer dans le body en 2 paramètre.