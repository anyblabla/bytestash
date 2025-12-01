# Nginx Proxy Manager compose - max v1_12_5

Fichier "docker-compose.yml" à utiliser pour déployer Nginx Proxy Manager via Docker.
Compose ok jusqu'à la version  1.12.5 !
Jusqu'à la version 1.12.3 tout fonctionne correctement.
Les versions 1.12.4 et 1.12.5 pourraient connaître un problème de lenteur au démarrage !
Problème réglé avec la version 1.12.6 disponible ici -> https://bytestash.blablalinux.be/s/8e6ca3b37473e7da0334d7669d18ff9c

• yml
• compose
• docker
• nginx
• npm

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
# Page de wiki disponible à cette adresse : https://wiki.blablalinux.be/fr/docker-compose-nginx-proxy-manager
services:
  npm:
    image: 'jc21/nginx-proxy-manager:latest'
    container_name: npm
    restart: always
    ports:
      # These ports are in format <host-port>:<container-port>
      - '80:80' # Public HTTP Port
      - '443:443' # Public HTTPS Port
      - '81:81' # Admin Web Port
      # Add any other Stream port you want to expose
      # - '21:21' # FTP

    environment:
      # Uncomment this if you want to change the location of
      # the SQLite DB file within the container
      # DB_SQLITE_FILE: "/data/database.sqlite"

      # Uncomment this if IPv6 is not enabled on your host
      # DISABLE_IPV6: 'true'

      # X-FRAME-OPTIONS Header
      X_FRAME_OPTIONS: "sameorigin"

    volumes:
      - ./data:/data
      - ./letsencrypt:/etc/letsencrypt
      - ./logrotate.custom:/etc/logrotate.d/nginx-proxy-manager

    healthcheck:
      test: ["CMD", "/usr/bin/check-health"]
      interval: 10s
      timeout: 3s

```
