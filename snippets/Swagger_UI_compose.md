# Swagger UI compose

Déploiement Docker de Swagger UI pour documenter une API externe (ex: LanguageTool). Inclut la gestion d'un fichier de spécification local via volume et la désactivation du validateur externe pour les environnements sécurisés derrière un reverse proxy.

• docker
• compose
• yml
• swagger
• api
• devtools

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
services:
  swagger-ui:
    image: swaggerapi/swagger-ui:latest
    container_name: swagger-ui
    restart: always
    ports:
      - "8085:8080"
    volumes:
      # Montage du dossier contenant languagetool-api.json
      - ./specs:/usr/share/nginx/html/specs
    environment:
      # Lien vers le fichier JSON local
      API_URL: "specs/languagetool-api.json"
      # Désactive le badge de validation externe (évite l'erreur INVALID si l'accès est restreint)
      VALIDATOR_URL: ""
```
