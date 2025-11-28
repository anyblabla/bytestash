# Nginx Proxy Manager compose (à partir de la v1.12.6)

Fichier "docker-compose.yml" à utiliser pour déployer Nginx Proxy Manager via Docker.
Compose ok à partir de la version  1.12.6 !

• docker
• compose
• yml
• nginx

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
# Page de wiki disponible à cette adresse : https://wiki.blablalinux.be/fr/docker-compose-nginx-proxy-manager
services:
  npm:
    image: jc21/nginx-proxy-manager:2.12.6
    container_name: npm
    restart: always
    ports:
      - 80:80
      - 443:443
      - 81:81
    environment:
      IP_RANGES_FETCH_ENABLED: false
      SKIP_CERTBOT_OWNERSHIP: true
    volumes:
      - ./data:/data
      - ./letsencrypt:/etc/letsencrypt
      - ./logrotate.custom:/etc/logrotate.d/nginx-proxy-manager
    healthcheck:
      test:
        - CMD
        - /usr/bin/check-health
      interval: 10s
      timeout: 3s

```
