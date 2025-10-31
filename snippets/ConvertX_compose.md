# ConvertX compose

Fichier "docker-compose.yml" à utiliser pour déployer ConvertX via Docker.

• docker
• compose
• yml
• convertx

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
services:
  convertx:
    image: ghcr.io/c4illin/convertx:latest
    container_name: convertx
    restart: always
    ports:
      - "3000:3000"
    environment:
      - JWT_SECRET= # https://jwtsecret.com/generate
      - ACCOUNT_REGISTRATION=true
      - HTTP_ALLOWED=false
      - ALLOW_UNAUTHENTICATED=false
      - AUTO_DELETE_EVERY_N_HOURS=24
      #- FFMPEG_ARGS=preset veryfast
      - HIDE_HISTORY=false
    volumes:
      - ./data:/app/data

```
