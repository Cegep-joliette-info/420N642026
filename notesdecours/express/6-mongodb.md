# MongoDB

MongoDB est une BD NoSQL. MongoDB est sous licence SSPL (licence open source développé par MongoDB, basé sur GPL3). Il s'agit d'une licence virale, si vous utilisez ce logiciel dans votre programme, ce dernier doit inclure aussi cette licence.

Pour installer MongoDB Community Edition, suivez la procédure sur [le site de MongoDB](https://docs.mongodb.com/manual/installation/). Je vous conseil aussi d'installer [Compass](https://www.mongodb.com/try/download/compass), c'est un GUI pour MongoDB.

## Structure
Une base de données contient des collections (comparable aux tables) qui contiennent des documents (comparable aux enregistrements). Chaque document est un JSON, une base de donnée peut être exporté en JSON. La structure des documents dans une collection est optionnelle, il n'y a pas de colonnes.

Lorsqu'on insère un nouveau document (un JSON je rappel), MongoDB ajoute automatiquement un id ( _id ) qui est un objet qui contient la propriété "$oid". Cet ID est créé avec le timestamp, un nombre aléatoire et d'un nombre incrémenté. De cette manière l'ID va être unique même si votre BD se retrouve sur plusieurs serveurs à cause du load-balancing. Exemple:

"_id": {"$oid": "6005f50416fd1aeeeb77e2ec"}
Vous pouvez forcer une certaine structure à vos documents dans une collection avec la validation. La validation est encore un JSON qui permet de définir: les champs obligatoires, les types des champs, etc. Voici un exemple tiré de [la documentation de MongoDB](https://docs.mongodb.com/compass/current/validation#validation-rules):

{
   $jsonSchema: {
      bsonType: "object",
      required: [ "name", "year", "major", "gpa", "address.city", "address.street" ],
      properties: {
         name: {
            bsonType: "string",
            description: "must be a string"
         },
         year: {
            bsonType: "int",
            minimum: 2017,
            maximum: 3017,
            exclusiveMaximum: false,
            description: "must be an integer in [ 2017, 3017 ]"
         },
         major: {
            bsonType: "string",
            enum: [ "Math", "English", "Computer Science", "History", null ],
            description: "can only be one of the enum values"
         },
         gpa: {
            bsonType: [ "double" ],
            minimum: 0,
            description: "must be a double"
         }
      }
   }
}
Bson est un mot inventé par MongoDB qui veut dire Binary JSON. Notez que c'est possible d'ignorer la validation de MongoDB avec l'option "bypassDocumentValidation". Cette option peut être disponible ou non aux utilisateurs, vérifiez vos ACL lorsque vous créez vos utilisateurs!

## Mongoose

Mongoose est une librairie qui simplifie l'interaction entre Node.js et MongoDB. Au lieu d'écrire des requêtes BD, on va exécuter des fonctions Mongoose qui va se charger de faire les bonnes actions. Installez "mongoose" avec npm dans votre serveur, créez un dossier config à la racine de votre serveur et créez un fichier db.js:

```ts
import mongoose from 'mongoose';
mongoose.connect('mongodb://localhost/nomDeBd');
```

Ensuite, importez le fichier 'config/db.js' dans votre index.js afin d'ouvrir la connexion à la base de donnée. Comme on a rien export dans le fichier de config, pas besoin de from dans le import.

```ts
import "./config/db";
```

Ensuite, créez un dossier "models" et créez un fichiers de model, user.js par exemple:

```js
import mongoose from 'mongoose';
const userSchema = new mongoose.Schema({
  username: String,
  password: String
});
const User = mongoose.model('User', userSchema);
export { User };
```

Un schéma est lié à une collection MongoDB, ça définis la forme des documents de cette collection. Dans l'exemple précédent, on aurait une collection User, chaque document aurait un username et un password. Ensuite avec le schéma on crée un modèle, le modèle est l'outil utilisé pour créer et consulter les documents de la collection. Maintenant on peut mettre à jour notre contrôleur avec mongoose, voici quelques exemples:

```js
import { User } from '../models/user';

// Trouver tous les users, 'users' est null s'il ne trouve rien
User.find().exec().then(users => {});

// Trouver un user, le username provient du body de l'appel POST
User.findOne({ username: req.body.username }).exec().then(user => {});

// Créer un user avec les infos passés dans le body de l'appel PUT
const newUser = new User(req.body);
newUser.save(err => {
  if (err) {
    // bad
  } else {
    // success
  }
});

// Mettre à jour un user
let user = await User.findOne({ username: 'toto' });
user.password = 'soleil 123';
user.save();
// OU
User.updateOne({ username: 'toto' }, { password: 'soleil123' });

// Supprimer un user
user.remove(err => {
  if (err) ...
});
// OU
User.deleteOne({username: 'toto'}, err => {});
```

Si la collection n'existe pas, elle sera créée.