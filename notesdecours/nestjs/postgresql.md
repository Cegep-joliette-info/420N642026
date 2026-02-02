# PostgreSQL


## Installation

Vous pouvez télécharger PostgreSQL à l'adresse: https://www.postgresql.org/download/. C'est disponible sur Homebrew pour MAC.

PostgreSQL inclue seulement le serveur, ça vous prend aussi un IDE pour aider à visualiser la BD. Vous pouvez utiliser [pgAdmin4](https://www.pgadmin.org/download/) ou [dbeaver](https://dbeaver.io/).

## Particularités

PostgreSQL peut être comparé à SQL Server, mais possède beaucoup plus de possibilités. Notez qu'au lieu du T-SQL c'est du pgSQL, qui est similaire.

Dans une base de données vous avez des schémas, ce sont des namespaces pour vos tables. Par défaut il y a un schéma `public`. Vous pouvez ignorer les schémas ou les utiliser, à votre guise.
Si vous avez 2 schémas (public et admin), dans chacun vous avez la table users. Si vous faites la requête `SELECT * FROM users` il prendra la table du schéma public par défaut. Vous pouvez faire `SELECT * FROM admin.users` pour forcer un schéma. Vous pouvez aussi changer l'ordre des schémas par défaut avec la commande `SET search_path TO admin, public;`.

PostgreSQL ne permet pas de faire du NoSQL, mais permet de stocker des JSON pour bénificier des avantages des BD NoSQL. Voici un exemple de création de table:

```sql
CREATE TABLE public.users (
	id bigserial PRIMARY KEY,
	"data" jsonb NOT NULL
);
```

  - Pour l'ID vous pouvez utiliser le type serial (int autoincrement), bigserial (bigint autoincrement) ou uuid (si vous avez plusieurs serveurs de BD).
  - Pour le JSON, vous avez le choix entre json (format texte, peu d'espace mais lent) et jsonb (format binaire, rapide mais prend plus d'espace).

Si vous voulez faire une relation entre 2 tables, certains sites conseilles de garder la FK à l'extérieur du JSON. Mais vous pouvez tout mettre dans le JSON incluant le PK si vous voulez!


## Requêtes JSON dans PostgreSQL

Exemples de: http://www.silota.com/docs/recipes/sql-postgres-json-data-types.html

```pgSQL
# INSERT
insert into sales values (1, '{name: "Alice", paying: true, tags: ["admin"]}');

# UPDATE, changer tout le JSON
update sales set info = '{name: "Bob", paying: false, tags: []}';

# UPDATE, ajouter ou modifier une partie du JSON
update sales set info = info || '{"country": "Canada"}';

# UPDATE, enlever une partie du JSON
update sales set info = info - 'country';
```

Pour fouiller dans le JSON en SQL, PostgreSQL a ajouté des symboles:

 - `->` pour obtenir une propriété en format JSON

```pgSQL
SELECT data->username FROM users;
```

 - `->>` pour obtenir une proriété en format texte

```pgSQL
select data from users where data->>'username' = 'admin';
```

 - pour obtenir une propriété numérique

```pgSQL
WHERE CAST ( data ->> 'qty' AS INTEGER) = 2
```

## Intégration avec NestJS et Sequelize

Exécutez les commandes suivantes:

```
npm install --save @nestjs/sequelize sequelize sequelize-typescript @sequelize/postgres pg pg-hstore
npm install --save-dev @types/sequelize sequelize-cli
npx sequelize-cli init
```

Modifiez le fichier `config/config.json` généré par Sequelize pour ajouter la configuration de la base de données PostgreSQL:

```json
{
  "development": {
    "username": "postgres",
    "password": null,
    "database": "atelier_development",
    "host": "127.0.0.1",
    "port": 5432,
    "dialect": "postgres"
  },
  "test": {
    "username": "postgres",
    "password": null,
    "database": "atelier_test",
    "host": "127.0.0.1",
    "port": 5432,
    "dialect": "postgres"
  },
  "production": {
    "username": "postgres",
    "password": null,
    "database": "atelier",
    "host": "127.0.0.1",
    "port": 5432,
    "dialect": "postgres"
  }
}
```

Si ça crée des conflits dans votre équipe, vous pouvez ajouter le fichier `config/config.json` au `.gitignore`. Dans ce cas on va créer un fichier `config/config_example.json` avec les mêmes données, et chaque développeur devra créer son propre `config/config.json` en se basant sur l'exemple.

Dans le fichier `app.module.ts`, ajoutez la configuration de Sequelize:

```ts
@Module({
  imports: [
      CatModule,
      SequelizeModule.forRoot({
        dialect: 'postgres',
        host: process.env.DB_HOST || '127.0.0.1',
        port: Number(process.env.DB_PORT) || 5432,
        username: process.env.DB_USER || 'postgres',
        password: process.env.DB_PASS || undefined,
        database: process.env.DB_NAME || 'atelier_development',
        autoLoadModels: true,
        models: [],
        repositoryMode: true,
        define: {
          timestamps: true,
          paranoid: true,
          underscored: true,
          updatedAt: "updated_at",
          createdAt: "created_at",
          deletedAt: "deleted_at",
        }
      })
  ],
  controllers: [AppController],
  providers: [AppService],
})
```

Il faut ajouter la ligne suivante à votre `tsconfig.json` pour que Sequelize fonctionne correctement avec TypeScript:

```json
"useDefineForClassFields": false
```

## Gestion des modèles

Voici un exemple de modèle:

```ts
import { DataTypes } from "sequelize";
import {
    AllowNull,
    AutoIncrement,
    Column,
    CreatedAt,
    DeletedAt,
    Model,
    PrimaryKey,
    Table,
    UpdatedAt
} from "sequelize-typescript";

@Table({ tableName: "cats" })
export class Cat extends Model {
    @AutoIncrement
    @PrimaryKey
    @Column(DataTypes.INTEGER)
    declare id: number;

    @AllowNull(false)
    @Column
    name!: string;

    @CreatedAt
    @Column({ field: "created_at" })
    declare createdAt?: Date;

    @UpdatedAt
    @Column({ field: "updated_at" })
    declare updatedAt?: Date;

    @DeletedAt
    @Column({ field: "deleted_at" })
    declare deletedAt?: Date;
}
```

Dans le `cat.module.ts`, importez le modèle:

```ts
import { SequelizeModule } from '@nestjs/sequelize';
import { Cat } from '../models/cats.model';

@Module({
  imports: [SequelizeModule.forFeature([Cat])],
  ...
})
```

Et il faut l'ajouter la le `app.module.ts` dans la liste des modèles de sequilize.

## Migrations

Les migrations permettent de versionner la structure de la base de données. Créez un dossier `migrations` à la racine de votre projet. Ensuite, créez un fichier de migration qui est nommé avec la date et le sujet de la migration, par exemple `20240601-create-cats-table.js`:

```ts
'use strict';

/** @type {import('sequelize-cli').Migration} */
module.exports = {
  async up (queryInterface, Sequelize) {
    await queryInterface.createTable('cats', {
      id: {
        type: Sequelize.INTEGER,
        allowNull: false,
        autoIncrement: true,
        primaryKey: true,
      },
      name: {
        type: Sequelize.STRING,
        allowNull: false,
      },
      created_at: {
        type: Sequelize.DATE,
        allowNull: false,
        defaultValue: Sequelize.literal('CURRENT_TIMESTAMP'),
      },
      updated_at: {
        type: Sequelize.DATE,
        allowNull: false,
        defaultValue: Sequelize.literal('CURRENT_TIMESTAMP'),
      },
      deleted_at: {
        type: Sequelize.DATE,
        allowNull: true,
      },
    });
  },

  async down (queryInterface, Sequelize) {
    await queryInterface.dropTable('cats');
  }
};

```

Ensuite il faut exécuter la migration avec la commande:

```
npx sequelize-cli db:migrate
```

## Service

Voici un exemple de service utilisant le modèle Cat:

```ts
export class CatService {
    constructor(
        @InjectModel(Cat)
        private catModel: typeof Cat,
    ) {}

    async create(createCatDto: CreateCatDto): Promise<Cat> {
        const cat = await this.catModel.create({ ...createCatDto });
        return cat;
    }

    // Attention, retourner l'objet complet peut inclure des données sensibles, on devrait transformer l'objet en entité Cat avant de le retourner
    async findAll(): Promise<Cat[]> {
        return this.catModel.findAll();
    }

    async findOne(id: number): Promise<Cat | null> {
        return this.catModel.findByPk(id);
    }
}
```

!!! À PARTIR D'ICI JE N'AI PAS EU LE TEMPS DE TOUT TESTER !!!

Commandes Sequelize pour interagir avec la bd:

```ts
// Créer un enregistrement
this.catModel.create({ name: 'Whiskers', age: 3 });

// Trouver tous les enregistrements
this.catModel.findAll();

// Trouver un enregistrement par sa clé primaire
this.catModel.findByPk(id);

// Trouver un enregistrement avec une condition
this.catModel.findOne({ where: { name: 'Whiskers' } });

// Mettre à jour des enregistrements
this.catModel.update({ age: 4 }, { where: { name: 'Whiskers' } });

// Supprimer des enregistrements
this.catModel.destroy({ where: { name: 'Whiskers' } });
```

```ts
// Récupérer un enregistrement puis le mettre à jour
const cat = await this.catModel.findByPk(id);
if (cat) {
    cat.name = 'NewName';
    await cat.save();
}
```

## Exemples de requêtes avancées

### Requêtes avec WHERE

```ts
// Trouver tous les chats avec un nom spécifique
const cats = await this.catModel.findAll({
    where: { name: 'Whiskers' }
});

// Trouver tous les chats avec un âge supérieur à 2
const olderCats = await this.catModel.findAll({
    where: { 
        age: { [Op.gt]: 2 } 
    }
});

// Conditions multiples (AND)
const cats = await this.catModel.findAll({
    where: { 
        name: 'Whiskers',
        age: { [Op.gte]: 3 }
    }
});

// Conditions avec OR
import { Op } from 'sequelize';

const cats = await this.catModel.findAll({
    where: {
        [Op.or]: [
            { name: 'Whiskers' },
            { name: 'Fluffy' }
        ]
    }
});

// Recherche avec LIKE
const cats = await this.catModel.findAll({
    where: {
        name: { [Op.like]: '%Whi%' }
    }
});

// Recherche avec IN
const cats = await this.catModel.findAll({
    where: {
        name: { [Op.in]: ['Whiskers', 'Fluffy', 'Shadow'] }
    }
});

// Requête avec NOT
const cats = await this.catModel.findAll({
    where: {
        name: { [Op.ne]: 'Whiskers' }
    }
});
```

### Requêtes brutes (Raw queries)

```ts
import { Sequelize } from 'sequelize-typescript';

export class CatService {
    constructor(
        @InjectModel(Cat)
        private catModel: typeof Cat,
        private sequelize: Sequelize,
    ) {}

    // Requête SQL brute simple
    async findCatsRaw(): Promise<any[]> {
        const [results, metadata] = await this.sequelize.query(
            'SELECT * FROM cats WHERE age > 2'
        );
        return results;
    }

    // Requête SQL brute avec paramètres (protection contre SQL injection)
    async findCatsByNameRaw(name: string): Promise<any[]> {
        const [results] = await this.sequelize.query(
            'SELECT * FROM cats WHERE name = :name',
            {
                replacements: { name },
                type: Sequelize.QueryTypes.SELECT
            }
        );
        return results;
    }

    // Requête complexe avec jointure
    async findCatsWithOwnersRaw(): Promise<any[]> {
        const [results] = await this.sequelize.query(`
            SELECT c.*, o.name as owner_name 
            FROM cats c 
            LEFT JOIN owners o ON c.owner_id = o.id 
            WHERE c.deleted_at IS NULL
            ORDER BY c.created_at DESC
        `, {
            type: Sequelize.QueryTypes.SELECT
        });
        return results;
    }

    // Insertion brute
    async createCatRaw(name: string, age: number): Promise<void> {
        await this.sequelize.query(
            'INSERT INTO cats (name, age, created_at, updated_at) VALUES (:name, :age, NOW(), NOW())',
            {
                replacements: { name, age },
                type: Sequelize.QueryTypes.INSERT
            }
        );
    }

    // Mise à jour brute
    async updateCatRaw(id: number, name: string): Promise<void> {
        await this.sequelize.query(
            'UPDATE cats SET name = :name, updated_at = NOW() WHERE id = :id',
            {
                replacements: { id, name },
                type: Sequelize.QueryTypes.UPDATE
            }
        );
    }

    // Requête avec agrégation
    async getCatStatistics(): Promise<any> {
        const [results] = await this.sequelize.query(`
            SELECT 
                COUNT(*) as total_cats,
                AVG(age) as average_age,
                MAX(age) as max_age,
                MIN(age) as min_age
            FROM cats
            WHERE deleted_at IS NULL
        `, {
            type: Sequelize.QueryTypes.SELECT
        });
        return results[0];
    }
}
```

## Jointures dans les modèles et les requêtes

### Modèles

Pour établir une relation entre les modèles, vous devez définir les associations dans vos classes de modèle. Par exemple, si vous avez un modèle `Owner` et un modèle `Cat`, vous pouvez définir une relation un-à-plusieurs comme suit:

```ts
import { Table, Column, Model, HasMany } from "sequelize-typescript";
import { Cat } from "./cats.model";

@Table({ tableName: "owners" })
export class Owner extends Model {
    @Column
    name!: string;

    @HasMany(() => Cat)
    cats!: Cat[];
}
```

### Requêtes avec jointures

Pour effectuer une jointure dans le service, vous pouvez utiliser l'option `include` de Sequelize. Voici un exemple de méthode dans le service `CatService` qui récupère les chats avec leurs propriétaires:

```ts
import { InjectModel } from '@nestjs/sequelize';
import { Cat } from '../models/cats.model';
import { Owner } from '../models/owners.model';

export class CatService {
    constructor(
        @InjectModel(Cat)
        private catModel: typeof Cat,
    ) {}

    async findCatsWithOwners(): Promise<Cat[]> {
        return this.catModel.findAll({
            include: [{
                model: Owner,
                required: false // false pour une jointure gauche
            }]
        });
    }
}
```

Dans cet exemple, la méthode `findCatsWithOwners` récupère tous les chats et inclut les informations sur leurs propriétaires, si disponibles.

### Exemple d'accès à l'Owner depuis le modèle Cat

Pour permettre à un modèle `Cat` d'accéder à son `Owner`, vous devez définir une relation dans le modèle `Cat`. Voici comment vous pouvez le faire:

```ts
import { Table, Column, Model, BelongsTo } from "sequelize-typescript";
import { Owner } from "./owners.model";

@Table({ tableName: "cats" })
export class Cat extends Model {
    @Column
    name!: string;

    @Column
    ownerId!: number; // Assurez-vous d'avoir une colonne pour stocker l'ID de l'Owner

    @BelongsTo(() => Owner)
    owner!: Owner; // Relation vers le modèle Owner
}
```

Avec cette configuration, vous pouvez accéder à l'Owner d'un Cat comme suit:

```ts
const cat = await this.catModel.findByPk(catId, {
    include: [Owner] // Inclure les informations sur l'Owner
});

console.log(cat.owner); // Accéder à l'Owner du Cat
```