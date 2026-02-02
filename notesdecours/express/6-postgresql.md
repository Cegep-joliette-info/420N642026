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

## Intégration avec Express

```js
const pgp = require('pg-promise')(/* options */)
const db = pgp('postgres://username:password@host:port/database')

db.one('SELECT $1 AS value', 123)
  .then((data) => {
    console.log('DATA:', data.value)
  })
  .catch((error) => {
    console.log('ERROR:', error)
  })
```

Exemple tiré de: https://expressjs.com/en/guide/database-integration.html#postgresql

Dans l'exemple, le `$1` est un paramètre ordonné, il sera remplacé par le `123` à l'exécution et protège contre l'injection SQL.

Pour plus d'exemple: https://github.com/vitaly-t/pg-promise

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

```pgSQLs
WHERE CAST ( data ->> 'qty' AS INTEGER) = 2
```
