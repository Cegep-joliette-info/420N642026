# Swagger

Swagger est un outil qui permet de documenter et tester une API REST de façon interactive. Avec NestJS, l'intégration de Swagger est facilitée grâce au module `@nestjs/swagger`.

## Installation

Pour installer Swagger dans votre projet NestJS, exécutez la commande suivante:

```bash
npm install --save @nestjs/swagger
```

Ensuite il faut le configurer dans le fichier principal `main.ts`. Voici un exemple de configuration de base (exemple de https://docs.nestjs.com/openapi/introduction#bootstrap):

```ts
import { NestFactory } from '@nestjs/core';
import { SwaggerModule, DocumentBuilder } from '@nestjs/swagger';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);

  const config = new DocumentBuilder()
    .setTitle('Cats example')
    .setDescription('The cats API description')
    .setVersion('1.0')
    .addTag('cats')
    .build();
  const documentFactory = () => SwaggerModule.createDocument(app, config);
  SwaggerModule.setup('api', app, documentFactory);

  await app.listen(process.env.PORT ?? 3000);
}
bootstrap();
```

Ensuite partez votre NestJS avec `nest start --watch` (ou avec le bouton play du package.json) et allez à l'URL `http://localhost:3000/api` pour voir l'interface Swagger.

Pour détailler les DTO, ajoutez des décorateurs aux classes DTO. Par exemple:

```ts
import {ApiProperty} from "@nestjs/swagger";

export class CreateCatDto {
    @ApiProperty()
    id: number;

    @ApiProperty()
    name: string;

    @ApiProperty()
    color: string;
}
```