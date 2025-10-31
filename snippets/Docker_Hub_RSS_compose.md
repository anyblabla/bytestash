# Docker Hub RSS compose

Fichier "docker-compose.yml" à utiliser pour déployer Docker Hub RSS via Docker.

• docker
• compose
• hub
• rss

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
services:
  docker-hub-rss:
    image: theconnman/docker-hub-rss:latest
    container_name: docker-hub-rss
    restart: always
    ports:
      - 3000:3000
```
