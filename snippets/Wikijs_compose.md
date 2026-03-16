# Wikijs compose

Fichier "docker-compose.yml" à utiliser pour déployer Wiki.js via Docker.

• compose
• yml
• docker
• wiki

```yaml
# =========================================================================
# WIKI.JS AVEC SYNCHRONISATION GIT (DOCKER COMPOSE)
# Auteur : Amaury aka BlablaLinux (https://link.blablalinux.be)
# =========================================================================
# Ce compose utilise des montages ciblés pour permettre les mises à jour
# automatiques du moteur via Watchtower tout en gardant vos données.
# =========================================================================

services:
  db:
    image: postgres:15-alpine
    container_name: postgres
    restart: always
    environment:
      POSTGRES_DB: wikidb             # <-- [1] À PERSONNALISER
      POSTGRES_PASSWORD: strong_pass  # <-- [2] À PERSONNALISER
      POSTGRES_USER: wikiuser         # <-- [3] À PERSONNALISER
    ports:
      - "5432:5432"
    volumes:
      - ./db-data:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U $$POSTGRES_USER -d $$POSTGRES_DB"]
      interval: 10s
      timeout: 5s
      retries: 5

  wiki:
    image: ghcr.io/requarks/wiki:2
    container_name: wiki
    restart: always
    depends_on:
      db:
        condition: service_healthy
    environment:
      DB_HOST: db
      DB_NAME: wikidb             # <-- DOIT CORRESPONDRE À [1]
      DB_PASS: strong_pass        # <-- DOIT CORRESPONDRE À [2]
      DB_PORT: 5432
      DB_TYPE: postgres
      DB_USER: wikiuser           # <-- DOIT CORRESPONDRE À [3]
    ports:
      - 80:3000
    volumes:
      - ./data/config.yml:/wiki/config.yml
      - ./data/repo:/wiki/data/repo
      - ./data/uploads:/wiki/data/uploads
      - ./data/secure:/wiki/data/secure
    healthcheck:
      test: ["CMD", "node", "-e", "require('http').get('http://localhost:3000/healthz', (res) => { if (res.statusCode === 200) process.exit(0); else process.exit(1); })"]
      interval: 30s
      timeout: 10s
      retries: 3

  wikijs-sitemap:
    image: hostwiki/wikijs-sitemap:latest
    container_name: sitemaps
    restart: always
    depends_on:
      db:
        condition: service_healthy
    environment:
      DB_HOST: db
      DB_NAME: wikidb             # <-- DOIT CORRESPONDRE À [1]
      DB_PASS: strong_pass        # <-- DOIT CORRESPONDRE À [2]
      DB_PORT: 5432
      DB_TYPE: postgres
      DB_USER: wikiuser           # <-- DOIT CORRESPONDRE À [3]
    ports:
      - 3012:3012
    healthcheck:
      test: ["CMD-SHELL", "ps aux | grep node || exit 1"]
      interval: 1m
      timeout: 10s
      retries: 3
```
