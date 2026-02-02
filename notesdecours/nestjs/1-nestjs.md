# NestJS

NestJS est un framework Node.js progressif pour la construction d'applications côté serveur efficaces, fiables et évolutives. Il utilise TypeScript par défaut et s'inspire des concepts d'Angular, ce qui le rend familier pour les développeurs front-end.

Il utilise express.js sous le capot, mais ajoute une structure modulaire, une injection de dépendances et d'autres fonctionnalités avancées pour faciliter le développement d'applications complexes.

## Installation

Avec npm, exécutez la commande suivante pour installer le CLI de NestJS globalement. Si vous ne l'avez pas encore fait, le `install -g` a besoin d'une config de plus dans le lab mac (voir notes [Angular](../angular/1-angular.md)).

```
npm install -g @nestjs/cli
```

Ensuite dans votre dossier de projet, exécutez la commande suivante pour créer un nouveau projet NestJS:

```
nest new api
```

Il vous sera demandé de choisir un gestionnaire de paquets (npm ou yarn). Choisissez npm puisque Angular utilise npm.

## Resource

Comme avec Angular, une commande permet de générer les fichiers importants. Pour nest, on va générer une ressource, par exemple pour générer la resource "cats", on exécute la commande suivante:

```
nest g resource cats
```

Cela va générer un dossier `cats` avec les fichiers suivants:
 - cats.controller.ts : Le contrôleur pour gérer les requêtes HTTP
 - cats.service.ts : Le service pour la logique métier
 - dto/create-cat.dto.ts : Un DTO pour la création de chats
 - dto/update-cat.dto.ts : Un DTO pour la mise à jour de chats
 - cats.module.ts : Le module qui regroupe le contrôleur et le service
 - entities/cat.entity.ts : Classe représentant un chat

Dans une logique LISP, le contrôleur gère les routes, les paramètres et verbe HTTP, etc. Le service contient la logique métier, comme interagir avec la base de données, valider les données et effectuer des calculs.

## Controleur

Voici le contrôleur généré par la commande précédente:

```ts
@Controller('cat') // Ce contrôleur gère les routes sous /cat
export class CatController {

  // On inject le CatService pour utiliser la logique métier
  constructor(private readonly catService: CatService) {}

  // Lorsqu'on reçoit une requête POST à /cat, on prend le corps de la requête ($_POST en PHP)
  @Post()
  create(@Body() createCatDto: CreateCatDto) {
    return this.catService.create(createCatDto);
  }

  // Lorsqu'on reçoit une requête GET à /cat
  @Get()
  findAll() {
    return this.catService.findAll();
  }

  // Lorsqu'on reçoit une requête GET à /cat/:id, id peut être n'importe quelle valeur, si id aurait été un number, une validation aurait été faite
  @Get(':id')
  findOne(@Param('id') id: string) {
    return this.catService.findOne(+id);
  }

  // Lorsqu'on reçoit une requête PATCH à /cat/:id, l'ID provient de l'URL et le corps contient les données à mettre à jour
  @Patch(':id')
  update(@Param('id') id: string, @Body() updateCatDto: UpdateCatDto) {
    return this.catService.update(+id, updateCatDto);
  }

  @Delete(':id')
  remove(@Param('id') id: string) {
    return this.catService.remove(+id);
  }
}
```

Par défaut les routes retourne des code 200 (`Success`), mais vous pouvez changer cela avec le décorateur `@HttpCode()`. Par exemple pour retourner un 201 (`Created`) ou un 204 (`No Content`), vous pouvez faire:

```ts
    @Post()
    @HttpCode(201)
    create(@Body() createCatDto: CreateCatDto) {
        return this.catService.create(createCatDto);
    }
```