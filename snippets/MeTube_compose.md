# MeTube compose

Fichier "docker-compose.yml" à utiliser pour déployer MeTube via Docker.

• yml
• docker
• metube
• download
• compose

```yaml
# Modifications apportées par Blabla Linux : https: //link.blablalinux.be
services:
  metube:
    image: ghcr.io/alexta69/metube:latest
    container_name: metube
    restart: always
    ports:
      - "8081:8081"
    environment:
      - AUDIO_DOWNLOAD_DIR=/downloads/audio
      - DELETE_FILE_ON_TRASHCAN=true
      #- YTDL_OPTIONS={"cookiefile":"/cookies/cookies.txt"}
    volumes:
      - ./downloads:/downloads
      #- ./cookies:/cookies
```
