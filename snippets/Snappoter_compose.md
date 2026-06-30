# Snappoter compose

SnapOtter est une solution open-source d'auto-hébergement dédiée au traitement de fichiers. Elle regroupe plus de 150 outils couvrant l'image, la vidéo, l'audio, les PDF et les documents bureautiques (conversion, compression, recadrage, filigranes, OCR, sous-titres, etc.). Le traitement s'effectue entièrement en local, aucun fichier n'est envoyé vers des serveurs externes. L'accès est protégé par authentification. SnapOtter repose sur une stack Docker composée de l'application Node.js, d'une base de données PostgreSQL et d'un broker Redis.

• docker
• compose
• yml
• file
• video
• audio
• pdf
• conversion
• compression

```yaml
# =========================================================================
# SNAPOTTER COMPOSE
# Auteur : Amaury aka BlablaLinux (https://blablalinux.be/mes-services-publics/)
# =========================================================================

services:
  snapotter:
    image: snapotter/snapotter:latest
    container_name: snapotter
    ports:
      # Port d'écoute de l'interface web et de l'API
      - "1349:1349"
    volumes:
      # Fichiers utilisateurs, modèles AI, venv Python (persistant)
      - ./snapotter/data:/data
      # Fichiers temporaires de traitement (nettoyé automatiquement)
      - ./snapotter/workspace:/tmp/workspace
    environment:
      # Exécution en root (LXC Debian root, volumes bind mount)
      - PUID=0
      - PGID=0

      # Authentification obligatoire — aucune inscription publique possible
      - AUTH_ENABLED=true
      - DEFAULT_USERNAME=admin
      # Utilisé uniquement lors de la première initialisation, sans effet ensuite
      # Un changement de mot de passe est forcé au premier login
      - DEFAULT_PASSWORD=CHANGE_ME

      # Connexion à la base de données PostgreSQL interne
      - DATABASE_URL=postgres://snapotter:POSTGRES_PASSWORD@postgres:5432/snapotter
      # Connexion au broker Redis interne
      - REDIS_URL=redis://redis:6379

      # Nécessaire pour récupérer les vraies IP derrière NPM (X-Forwarded-For)
      - TRUST_PROXY=true
      # Restreindre les appels API CORS à ce seul domaine
      - CORS_ORIGIN=https://snapotter.example.com

      # Limite de requêtes API par IP et par minute (anti-abus public)
      - RATE_LIMIT_PER_MIN=60
      # Taille maximale par fichier uploadé
      - MAX_UPLOAD_SIZE_MB=100
      # Nombre maximum de fichiers par requête batch
      - MAX_BATCH_SIZE=20
      # Nombre de jobs AI parallèles (adapté aux ressources du serveur)
      - CONCURRENT_JOBS=2
      # Délai maximum de traitement par job avant timeout
      - PROCESSING_TIMEOUT_S=300
      # Durée de conservation des fichiers traités avant nettoyage automatique
      - FILE_MAX_AGE_HOURS=24
      # Nombre maximum de comptes utilisateurs (0 = illimité)
      - MAX_USERS=0
      - LOG_LEVEL=info
    depends_on:
      postgres:
        condition: service_healthy
      redis:
        condition: service_healthy
    restart: always
    healthcheck:
      # curl est disponible dans l'image, wget ne l'est pas
      test: ["CMD-SHELL", "curl -sf http://127.0.0.1:1349/api/v1/health || exit 1"]
      interval: 10s
      timeout: 5s
      retries: 12

  postgres:
    image: postgres:17-alpine
    container_name: snapotter-postgres
    ports:
      # Exposé uniquement en local pour les sauvegardes (ex: Databasus)
      - "127.0.0.1:5432:5432"
    environment:
      POSTGRES_USER: snapotter
      POSTGRES_PASSWORD: POSTGRES_PASSWORD
      POSTGRES_DB: snapotter
    volumes:
      # Données PostgreSQL (persistant — critique)
      - ./snapotter/pgdata:/var/lib/postgresql/data
    restart: always
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U snapotter"]
      interval: 10s
      timeout: 5s
      retries: 12

  redis:
    image: redis:8-alpine
    container_name: snapotter-redis
    # noeviction : Redis rejette les nouvelles écritures si plein (pas de purge silencieuse)
    # appendonly : persistance des jobs sur disque
    command: ["redis-server", "--maxmemory-policy", "noeviction", "--appendonly", "yes"]
    volumes:
      # Persistance Redis (file de jobs)
      - ./snapotter/redisdata:/data
    restart: always
    healthcheck:
      test: ["CMD", "redis-cli", "ping"]
      interval: 10s
      timeout: 5s
      retries: 12
```

```plaintext
# Configuration Nginx (bloc Advanced NPM) 

# Désactive l'en-tête révélant la technologie backend
proxy_hide_header X-Powered-By;

# Sécurité navigateur
add_header Referrer-Policy "no-referrer" always;
add_header X-Frame-Options SAMEORIGIN always;
add_header X-Xss-Protection "1; mode=block" always;

# Empêche l'indexation par les moteurs de recherche
add_header X-Robots-Tag "noindex, noarchive, nofollow" always;

# Obligatoire pour les SSE (progression batch, outils AI, installation de bundles)
proxy_buffering off;

# WebSocket géré nativement par l'option NPM "Prise en charge de Websockets"
```
