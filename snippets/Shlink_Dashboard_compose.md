# Shlink Dashboard compose

Déploiement complet de Shlink Dashboard avec son propre backend Node.js et une base de données PostgreSQL dédiée. Cette configuration inclut un correctif de healthcheck réseau TCP natif (via Node.js) pour valider l'état du conteneur sans dépendre de curl ou wget (absents de l'image officielle). La base de données est accessible extérieurement pour permettre le monitoring et l'indexation automatique par Databasus.

• docker
• compose
• yml
• dashboard
• short
• url
• openwebui

```yaml
# =========================================================================
# SHLINK DASHBOARD COMPOSE
# Auteur : Amaury aka BlablaLinux (https://link.blablalinux.be)
# =========================================================================

services:
  # --- Shlink Dashboard ---
  shlink_dashboard:
    container_name: shlink_dashboard
    image: shlinkio/shlink-dashboard:latest
    restart: always
    ports:
      - "3005:3005"
    environment:
      - NODE_ENV=production
      - SHLINK_DASHBOARD_PORT=3005
      
      # Connexion Base de données
      - SHLINK_DASHBOARD_DB_DRIVER=postgres
      - SHLINK_DASHBOARD_DB_HOST=shlink_dashboard_db_postgres
      - SHLINK_DASHBOARD_DB_PORT=5432
      - SHLINK_DASHBOARD_DB_NAME=shlink_dashboard_db
      - SHLINK_DASHBOARD_DB_USER=shlink_user
      - SHLINK_DASHBOARD_DB_PASSWORD=Changer_Moi_Le_Mot_De_Passe_Secret_123!
    healthcheck:
      # Test réseau natif via socket TCP pour contourner l'absence de curl/wget dans l'image officielle
      test: ["CMD-SHELL", "node -e \"const s = require('net').connect(3005, '127.0.0.1', () => process.exit(0)); s.on('error', () => process.exit(1));\""]
      interval: 30s
      timeout: 10s
      retries: 3
    depends_on:
      shlink_dashboard_db_postgres:
        condition: service_healthy

  # --- Base de données PostgreSQL ---
  shlink_dashboard_db_postgres:
    container_name: shlink_dashboard_db_postgres
    image: postgres:16-alpine
    restart: always
    ports:
      - "5432:5432"
    volumes:
      - ./data/database_pg:/var/lib/postgresql/data
    environment:
      - TZ=Europe/Brussels
      - POSTGRES_DB=shlink_dashboard_db
      - POSTGRES_USER=shlink_user
      - POSTGRES_PASSWORD=Changer_Moi_Le_Mot_De_Passe_Secret_123!
      - PGDATA=/var/lib/postgresql/data/pgdata
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U shlink_user -d shlink_dashboard_db"]
      interval: 5s
      timeout: 5s
      retries: 10
```
