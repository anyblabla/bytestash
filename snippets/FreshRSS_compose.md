# FreshRSS compose

Fichier "docker-compose.yml" à utiliser pour déployer FreshRSS via Docker.

• docker
• compose
• yml
• rss
• freshrss
• flux

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
services:
  freshrss:
    image: freshrss/freshrss:latest
    container_name: freshrss
    hostname: freshrss
    restart: always
    logging:
      options:
        max-size: 10m
    volumes:
      - ./data:/var/www/FreshRSS/data
      - ./extensions:/var/www/FreshRSS/extensions
    environment:
      TZ: Europe/Brussels
      CRON_MIN: 3,33
      TRUSTED_PROXY: 172.16.0.1/12 192.168.0.1/16
    healthcheck:
      test:
        - CMD
        - cli/health.php
      timeout: 10s
      start_period: 60s
      start_interval: 11s
      interval: 75s
      retries: 3
```
