# Shlink Server compose

Ce déploiement Docker Compose permet d'installer le cœur de Shlink, un réducteur d'URL auto-hébergé puissant. Cette configuration inclut l'application, une base de données PostgreSQL 16 et un cache Redis 7 pour des performances optimales.

Note MaxMind (Géo-localisation) :
Pour que Shlink puisse localiser les clics (pays, ville), vous devez obtenir une clé de licence gratuite GeoLite2 chez MaxMind (https://www.maxmind.com/). Une fois obtenue, placez-la dans la variable GEOLITE_LICENSE_KEY du fichier compose.

Comment générer la clé API ?
Une fois le conteneur lancé et opérationnel, vous devez générer une clé pour connecter votre interface d'administration. Lancez la commande suivante dans votre terminal :

docker compose exec shlink_app shlink api-key:generate

Conservez précieusement la clé affichée, elle ne sera plus visible par la suite.

• docker
• compose
• yml
• shlink
• shorter
• url
• file-server

```yaml
# =========================================================================
# SHLINK SERVER COMPOSE
# Auteur : Amaury aka BlablaLinux (https://link.blablalinux.be)
# =========================================================================

services:
  # --- Application Shlink (Moteur de réduction d'URL) ---
  shlink_app:
    container_name: shlink_app
    image: shlinkio/shlink:stable
    restart: always
    ports:
      - "8008:8080" # Port interne 8080 exposé sur le 8008 du LXC/Hôte
    volumes:
      - ./data/app:/home/shlink/data
    environment:
      # Paramètres Généraux
      - DEFAULT_DOMAIN=votre-domaine-court.tld # Remplacez par votre domaine (ex: sh.domaine.fr)
      - IS_HTTPS_ENABLED=true
      - GEOLITE_LICENSE_KEY=VOTRE_CLE_MAXMIND  # Clé GeoLite2 gratuite pour la géolocalisation des clics
      - TIMEZONE=Europe/Brussels
      - CACHE_NAMESPACE=Shlink
      - TRUSTED_PROXIES=192.168.x.x           # IP de votre Reverse Proxy (ex: Nginx Proxy Manager)
      - MEMORY_LIMIT=512M
      - LOGS_FORMAT=console

      # Configuration des liens
      - DEFAULT_SHORT_CODES_LENGTH=5
      - DELETE_SHORT_URL_THRESHOLD=100
      - AUTO_RESOLVE_TITLES=true
      - MULTI_SEGMENT_SLUGS_ENABLED=true
      - SHORT_URL_TRAILING_SLASH=true
      - SHORT_URL_MODE=strict

      # Connexion Base de données
      - DB_DRIVER=postgres
      - DB_NAME=shlink_prod
      - DB_USER=shlink_admin
      - DB_PASSWORD=votre_mot_de_passe_securise
      - DB_HOST=shlink_db
      - DB_PORT=5432
      - DB_USE_ENCRYPTION=false

      # Redirections par défaut
      - REDIRECT_CACHE_LIFETIME=30
      - REDIRECT_EXTRA_PATH_MODE=default
      - DEFAULT_BASE_URL_REDIRECT=https://votre-site.com
      - DEFAULT_INVALID_SHORT_URL_REDIRECT=https://fallback.votre-site.com
      - DEFAULT_REGULAR_404_REDIRECT=https://fallback.votre-site.com

      # Suivi des visites (Tracking)
      - DISABLE_TRACKING=false
      - TRACK_ORPHAN_VISITS=true
      - DISABLE_IP_TRACKING=false
      - DISABLE_REFERRER_TRACKING=false
      - DISABLE_UA_TRACKING=false
      - ANONYMIZE_REMOTE_ADDR=false

      # Indexation
      - ROBOTS_ALLOW_ALL_SHORT_URLS=false
      - ROBOTS_USER_AGENTS=*

      # CORS (Indispensable pour l'utilisation d'une interface d'administration distante)
      - CORS_ALLOW_ORIGIN=*
      - CORS_ALLOW_CREDENTIALS=false
      - CORS_MAX_AGE=3600

      # Intégration du cache Redis
      - REDIS_SERVERS=tcp://shlink_redis:6379

      # Configuration des QR Codes générés
      - DEFAULT_QR_CODE_SIZE=300
      - DEFAULT_QR_CODE_MARGIN=0
      - DEFAULT_QR_CODE_FORMAT=png
      - DEFAULT_QR_CODE_ERROR_CORRECTION=l
      - DEFAULT_QR_CODE_ROUND_BLOCK_SIZE=true
      - QR_CODE_FOR_DISABLED_SHORT_URLS=true
      - DEFAULT_QR_CODE_COLOR=#000000
      - DEFAULT_QR_CODE_BG_COLOR=#FFFFFF

    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:8080/rest/v3/health"]
      interval: 30s
      timeout: 10s
      retries: 3
    depends_on:
      shlink_db:
        condition: service_healthy
      shlink_redis:
        condition: service_healthy

  # --- Base de données PostgreSQL ---
  shlink_db:
    container_name: shlink_db
    image: postgres:16-alpine
    restart: always
    ports:
      - "5432:5432" # Port exposé pour administration externe (ex: Datagrip)
    environment:
      - TZ=Europe/Brussels
      - POSTGRES_DB=shlink_prod
      - POSTGRES_USER=shlink_admin
      - POSTGRES_PASSWORD=votre_mot_de_passe_securise
    volumes:
      - ./data/postgres:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U shlink_admin -d shlink_prod"]
      interval: 5s
      timeout: 5s
      retries: 10

  # --- Cache Redis ---
  shlink_redis:
    container_name: shlink_redis
    image: redis:7-alpine
    restart: always
    environment:
      - TZ=Europe/Brussels
    healthcheck:
      test: ["CMD", "redis-cli", "ping"]
      interval: 5s
      timeout: 5s
      retries: 5
```
