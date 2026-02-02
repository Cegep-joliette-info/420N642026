# Validation

NestJS utilise la bibliothèque `class-validator` pour la validation des données. Voici comment configurer et utiliser la validation dans une application NestJS.

## Installation

Installez les packages nécessaires:

```bash
npm install class-validator class-transformer
```

## Configuration

Dans le fichier `main.ts`, configurez le pipe de validation global:

```ts
async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  // Nouvelle ligne ajoutée pour la validation
  app.useGlobalPipes(
    new ValidationPipe({
            transform: true, // Convertit les types primitifs
            whitelist: true, // Supprime les propriétés non définies dans le DTO
        }
    )
);
  await app.listen(process.env.PORT ?? 3000);
}
bootstrap();
```

## Utilisation

Il faut ajouter des décorateurs de validation dans vos DTOs (Data Transfer Objects). Par exemple:

```ts

import { IsEmail, IsNotEmpty } from 'class-validator';

export class CreateUserDto {
  @IsEmail()
  email: string;

  @IsNotEmpty()
  password: string;
}
```

La liste complète des décorateurs de validation est disponible dans la [documentation officielle de class-validator](https://github.com/typestack/class-validator#validation-decorators).

Les messages d'erreurs sont en anglais, pour les traduire il faut ajouter les messages personnalisés dans les décorateurs, par exemple:

```ts
import { MinLength, MaxLength } from 'class-validator';

export class Post {
  @MinLength(10, {
    message: 'Le titre est trop court',
  })
  @MaxLength(50, {
    message: 'Le titre est trop long',
  })
  title: string;
}
```

Et c'est tout, si la validation échoue, NestJS renverra automatiquement une réponse HTTP 400 avec les détails des erreurs avant d'entrer dans votre contrôleur.