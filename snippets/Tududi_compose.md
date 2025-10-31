# Tududi compose

Fichier "docker-compose.yml" à utiliser pour déployer Tududi via Docker.

• docker
• compose
• yml
• task
• note
• tududi

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
services:
  tududi:
    image: chrisvel/tududi:latest
    container_name: tududi
    environment:
      - TUDUDI_USER_EMAIL= # votre adresse mail
      - TUDUDI_USER_PASSWORD= # votre mot de passe
      - TUDUDI_SESSION_SECRET= # votre clé secrète générée avec openssl rand -hex 64
      - TUDUDI_ALLOWED_ORIGINS= # votre domaine
      - TUDUDI_UPLOAD_PATH="/app/backend/uploads"
    volumes:
      - ./tududi_db:/app/backend/db
      - ./uploads:/app/backend/uploads
    ports:
      - 3002:3002
    restart: always
```
