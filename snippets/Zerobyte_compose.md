# Zerobyte compose

Fichier "docker-compose.yml" à utiliser pour déployer Zerobyte via Docker.

• docker
• compose
• yml
• backup
• sauvegarde
• zerobyte

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
# Page de projet GitHub : https://github.com/nicotsx/zerobyte
services:
  zerobyte:
    image: ghcr.io/nicotsx/zerobyte:latest
    container_name: zerobyte
    restart: always
    cap_add:
      - SYS_ADMIN
    ports:
      - "4096:4096"
    devices:
      - /dev/fuse:/dev/fuse
    environment:
      - TZ=Europe/Brussels  # Set your timezone here
    volumes:
      - /etc/localtime:/etc/localtime:ro
      - ./zerobyte:/var/lib/zerobyte
      - ./mydata:/mydata
```
