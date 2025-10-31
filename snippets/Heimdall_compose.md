# Heimdall compose

Fichier "docker-compose.yml" à utiliser pour déployer Heimdall via Docker.

• yml
• docker
• heimdall
• dashboard
• compose

```yaml
# Modifications apportées par Blabla Linux : https: //link.blablalinux.be
services:
  heimdall:
    image: lscr.io/linuxserver/heimdall:latest
    container_name: heimdall
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/Brussels
    volumes:
      - ./config:/config
    ports:
      - 80:80
      - 443:443
    restart: always
```
