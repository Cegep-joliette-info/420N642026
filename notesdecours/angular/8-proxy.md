# Proxy

*À supprimer*

Vous pouvez configurer un proxy afin de régler les problèmes de cors et renforcer la sécurité des cookies. Pour configurer le proxy, créez le fichier dans votre client `src/proxy.conf.json`  et y mettre le contenu:

```json
{
  "/api": {
    "target": "http://localhost:3000",
    "secure": false,
    "pathRewrite": {
      "^/api": ""
    }
  }
}
```

Ensuite, dans le fichier angular.json à la racine de votre client, ajoutez la propriété suivante dans l'objet "projects.nomDuProjet.architect.serve.options":

`"proxyConfig": "src/proxy.conf.json"`

Finalement, tous vos appels d'api vont maintenant pointer vers "/api" au lieu de "http://localhost:3000"