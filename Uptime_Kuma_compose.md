# Uptime Kuma compose

Fichier "docker-compose.yml" à utiliser pour déployer Uptime Kuma via Docker.

• docker
• compose
• yml
• uptime

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
services:
  uptime-kuma:
    image: louislam/uptime-kuma:latest
    container_name: uptime-kuma
    restart: always
    ports:
      - 3001:3001
    volumes:
      - ./data:/app/data
      - /var/run/docker.sock:/var/run/docker.sock
    environment:
      - Europe/Brussels
      - kuma_network
    healthcheck:
      test:
        - CMD
        - curl
        - -f
        - http://localhost:3001
      interval: 30s
      retries: 3
      start_period: 10s
      timeout: 5s
    logging:
      driver: json-file
      options:
        max-size: 10m
        max-file: "3"
networks:
  kuma_network:
    driver: bridge
```
