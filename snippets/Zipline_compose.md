# Zipline compose

Ce déploiement Docker Compose permet de mettre en place Zipline, un hébergeur de fichiers et d'images (SaaS-like) léger et performant. La configuration inclut une base de données PostgreSQL 16 avec des tests de santé (healthchecks) robustes pour garantir que Zipline ne démarre que lorsque la base de données est prête et opérationnelle.

• docker
• compose
• yml
• zipline
• postgresql
• sharing
• file-server

```yaml
# =========================================================================
# ZIPLINE COMPOSE
# Auteur : Amaury aka BlablaLinux (https://link.blablalinux.be)
# -------------------------------------------------------------------------

services:
  postgresql:
    image: postgres:16
    container_name: postgres
    restart: always
    environment:
      POSTGRES_USER: postgres # <-- À PERSONNALISER (ex: zipline_user)
      POSTGRES_PASSWORD: password_ici # <-- À MODIFIER (Mot de passe fort)
      POSTGRES_DB: zipline_db # <-- À PERSONNALISER (ex: zipline)
    volumes:
      - ./pgdata:/var/lib/postgresql/data
    ports:
      - "5432:5432" # <-- Utile si tu veux y accéder via un client externe
    healthcheck:
      test: ["CMD", "pg_isready", "-U", "postgres"] # <-- Adapter l'utilisateur si changé plus haut
      interval: 10s
      timeout: 5s
      retries: 5

  zipline:
    image: ghcr.io/diced/zipline:latest
    container_name: zipline
    restart: always
    ports:
      - 3000:3000 # <-- Port de l'interface web
    environment:
      # Modifie l'URL ci-dessous avec les infos PostgreSQL définies plus haut
      - DATABASE_URL=postgres://postgres:password_ici@postgresql:5432/zipline_db
      - CORE_HOSTNAME=0.0.0.0
      - CORE_SECRET=secret_aleatoire_ici # <-- À CHANGER (Génère une chaîne aléatoire)
    depends_on:
      postgresql:
        condition: service_healthy
    volumes:
      - ./uploads:/zipline/uploads
      - ./public:/zipline/public
      - ./themes:/zipline/themes
    healthcheck:
      # Test de connexion interne sur le port 3000 (Node.js)
      test: ["CMD-SHELL", "node -e \"require('net').createConnection(3000, '127.0.0.1').on('error', () => process.exit(1)).on('connect', () => process.exit(0))\""]
      interval: 10s
      timeout: 5s
      retries: 3
      start_period: 20s
```
