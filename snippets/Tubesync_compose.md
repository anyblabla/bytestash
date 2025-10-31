# Tubesync compose

Fichier "docker-compose.yml" à utiliser pour déployer Tubesync via Docker.

• compose
• yml
• docker
• tubesync
• download

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
services:
  tubesync:
    image: ghcr.io/meeb/tubesync:latest
    container_name: tubesync
    restart: unless-stopped
    ports:
      - 4848:4848
    volumes:
      - ./config:/config
      - ./downloads:/downloads
    environment:
      - TZ=Europe/Brussels
      #- PUID=1000
      #- PGID=1000
```
