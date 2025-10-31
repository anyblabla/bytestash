# Termix compose

Fichier "docker-compose.yml" à utiliser pour déployer Termix via Docker.

• shell
• terminal
• termix
• ssh
• yml
• compose
• docker

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
services:
  app:
    image: ghcr.io/lukegus/termix:latest
    container_name: termix-app
    restart: unless-stopped
    ports:
      - "8080:8080" # Port for nginx web-app (frontend)
#     - "8081:8081" # Port for SSH WebSocket, does not need to be open (backend)
```
