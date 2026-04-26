# Etherpad Lite compose

Fichier "docker-compose.yml" à utiliser pour déployer Etherpad Lite via Docker.

• docker
• compose
• yml
• etherpad

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
services:
  # --- Service Etherpad ---
  app:
    user: "0:0"
    image: etherpad/etherpad:latest
    container_name: etherpad
    tty: true
    stdin_open: true
    restart: always
    depends_on:
      postgres:
        condition: service_healthy  # L'app attend que la DB soit totalement opérationnelle
    environment:
      NODE_ENV: production
      ADMIN_PASSWORD: ${APP_ADMIN_PASSWORD:-admin}           # Mot de passe admin interface web
      DB_CHARSET: ${APP_DB_CHARSET:-utf8mb4}
      DB_HOST: postgres                                      # Nom du service de la base de données
      DB_NAME: ${DB_DATABASE:-etherpad}                      # Nom de la base de données
      DB_PASS: ${DB_PASSWORD:-admin}                         # Mot de passe utilisateur DB
      DB_PORT: ${DB_PORT:-5432}
      DB_TYPE: "postgres"
      DB_USER: ${DB_USER:-admin}                             # Nom utilisateur DB
      DEFAULT_PAD_TEXT: ${APP_DEFAULT_PAD_TEXT:- }           # Texte par défaut des nouveaux pads
      DISABLE_IP_LOGGING: ${APP_DISABLE_IP_LOGGING:-false}
      SOFFICE: ${APP_SOFFICE:-null}                          # Export PDF/LibreOffice (optionnel)
      TRUST_PROXY: ${APP_TRUST_PROXY:-true}                  # À garder vrai si derrière un reverse proxy
    ports:
      - "${APP_PORT_PUBLISHED:-9001}:${APP_PORT_TARGET:-9001}"
    volumes:
      - ./plugins:/opt/etherpad-lite/src/plugin_packages     # Stockage des plugins installés
      - ./etherpad-var:/opt/etherpad-lite/var                # Données variables de l'application

  # --- Service Base de données ---
  postgres:
    image: postgres:15-alpine
    container_name: postgres
    restart: always
    environment:
      POSTGRES_DB: ${DB_DATABASE:-etherpad}
      POSTGRES_PASSWORD: ${DB_PASSWORD:-admin}
      POSTGRES_PORT: ${DB_PORT:-5432}
      POSTGRES_USER: ${DB_USER:-admin}
      PGDATA: /var/lib/postgresql/data/pgdata                # Chemin interne des données PG
    ports:
      - "5432:5432"                                          # Exposition du port pour accès externe
    volumes:
      - ./postgres_data:/var/lib/postgresql/data             # Persistance des données SQL
    healthcheck:
      # Vérifie si Postgres est prêt avant de lancer Etherpad
      test: ["CMD-SHELL", "pg_isready -U $${POSTGRES_USER} -d $${POSTGRES_DB}"]
      interval: 10s
      timeout: 5s
      retries: 5
```

```plaintext
# --- CONFIGURATION RÉSEAU ---
# Port exposé sur l'hôte (ex: 9001)
APP_PORT_PUBLISHED=9001
# Port interne du conteneur Etherpad
APP_PORT_TARGET=9001

# --- CONFIGURATION ETHERPAD ---
# Texte d'accueil affiché à la création d'un nouveau pad
APP_DEFAULT_PAD_TEXT="Bienvenue sur Etherpad !"
# Mot de passe pour l'interface d'administration (/admin)
APP_ADMIN_PASSWORD=ChangerCeMotDePasse
# Encodage de la base de données
APP_DB_CHARSET=utf8mb4
# Désactiver le log des adresses IP (true/false)
APP_DISABLE_IP_LOGGING=false
# Faire confiance au reverse proxy (Nginx/Traefik)
APP_TRUST_PROXY=true

# --- CONFIGURATION BASE DE DONNÉES (POSTGRES) ---
# Nom de la base de données
DB_DATABASE=etherpad_db
# Nom de l'utilisateur de la base de données
DB_USER=etherpad_user
# Mot de passe de la base de données
DB_PASSWORD=MotDePasseBaseDeDonneesSecurise
# Port interne Postgres
DB_PORT=5432
```
