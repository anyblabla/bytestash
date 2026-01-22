# Termix compose

Ce fichier docker-compose.yml permet de déployer Termix, un terminal web léger et performant. Cette configuration inclut la persistance des données dans un volume local et expose l'interface sur le port 8080. Idéal pour accéder à un terminal via un navigateur sur vos serveurs Debian, Ubuntu ou via une instance Proxmox.

• docker
• compose
• yml
• termix
• shell
• web

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
services:
  termix:
    image: ghcr.io/lukegus/termix:latest
    container_name: termix
    restart: always
    ports:
      - 8080:8080
    volumes:
      - ./termix-data:/app/data
    environment:
      PORT: "8080"
volumes:
  termix-data:
    driver: local
```
