# Kanban Kan compose

Stack Docker Compose pour Kan – Alternative moderne à Wekan

Ce snippet permet de déployer l'outil de gestion de projet Kan avec une base PostgreSQL, un cache Redis et un support S3 pour le stockage.

Inclus dans cette conf :

Sécurité : Gestion des domaines mail autorisés et authentification par Lien Magique.
Admin Sys : Optimisé pour Databasus (Postgres configuré pour les accès distants).
Fiabilité : Conteneur de migration automatique et Healthchecks intégrés.

Usage : Adaptez le env.example, lancez docker compose up -d et profitez !

• docker
• compose
• yml
• kanban
• task
• kan
• s3

```yaml
# =========================================================================
# KAN COMPOSE
# Auteur : Amaury aka BlablaLinux (https://link.blablalinux.be)
# =========================================================================

services:
  migrate:
    image: ghcr.io/kanbn/kan-migrate:latest
    container_name: kan-migrate
    networks:
      - kan-network
    env_file:
      - .env
    environment:
      # Construction de l'URL de connexion DB via le réseau interne Docker
      - POSTGRES_URL=postgresql://kan:${POSTGRES_PASSWORD}@kan-db:5432/kan_db
    depends_on:
      postgres:
        condition: service_healthy
    restart: "no"

  web:
    image: ghcr.io/kanbn/kan:latest
    container_name: kan-web
    networks:
      - kan-network
    ports:
      - "3000:3000"
    env_file:
      - .env
    environment:
      - POSTGRES_URL=postgresql://kan:${POSTGRES_PASSWORD}@kan-db:5432/kan_db
      - REDIS_URL=redis://kan-redis:6379
    depends_on:
      migrate:
        condition: service_completed_successfully
      redis:
        condition: service_started
    restart: always

  postgres:
    image: postgres:15
    container_name: kan-db
    networks:
      - kan-network
    environment:
      - POSTGRES_DB=kan_db
      - POSTGRES_USER=kan
      - POSTGRES_PASSWORD=${POSTGRES_PASSWORD}
    ports:
      - "5432:5432" # Port exposé pour la gestion via Databasus
    # Force Postgres à écouter sur toutes les interfaces pour l'accès externe
    command: postgres -c listen_addresses='*'
    volumes:
      - ./db_data:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U kan -d kan_db"]
      interval: 5s
      timeout: 5s
      retries: 10
    restart: always

  redis:
    image: redis:alpine
    container_name: kan-redis
    networks:
      - kan-network
    volumes:
      - ./redis_data:/data
    restart: always

networks:
  kan-network:
    driver: bridge
```

```plaintext
# Configuration Générale
NODE_ENV=production
NEXTAUTH_URL=https://kan.ton-domaine.be

# Base de données (Utilisé dans le compose)
POSTGRES_PASSWORD=TonMotDePasseTresSecurise

# Authentification (Better-Auth)
# Ajoute bien tes domaines mail ici
BETTER_AUTH_SECRET=UnSecretGenereAleatoirement
BETTER_AUTH_ALLOWED_DOMAINS=gmail.com,outlook.com,outlook.be,ton-domaine.be

# Configuration Email (SMTP) pour les liens magiques
SMTP_HOST=mail.ton-serveur.com
SMTP_PORT=587
SMTP_USER=contact@ton-domaine.be
SMTP_PASS=TonMotDePasseSMTP
SMTP_FROM=contact@ton-domaine.be

# Stockage Objet S3 (Pour les pièces jointes)
S3_ENABLED=true
S3_ENDPOINT=https://s3.ton-fournisseur.com
S3_REGION=eu-central-1
S3_ACCESS_KEY_ID=TaCleAccessS3
S3_SECRET_ACCESS_KEY=TaCleSecreteS3
S3_BUCKET_NAME=nom-du-bucket
S3_PUBLIC_URL=https://cdn.ton-domaine.be
```
