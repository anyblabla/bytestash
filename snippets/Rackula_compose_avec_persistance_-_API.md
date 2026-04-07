# Rackula compose avec persistance - API

Déploiement complet de Rackula avec persistance des données sur le serveur (LXC/VM). Cette version utilise l'image persist et une API dédiée pour sauvegarder vos schémas de baies directement sur le disque du système hôte.

• docker
• compose
• yml
• rackula
• lxc
• persitence

```yaml
# =========================================================================
# RACKULA COMPOSE (Version Persistance API)
# Auteur : Amaury aka BlablaLinux (https://link.blablalinux.be)
# -------------------------------------------------------------------------
# Note : Nécessite un fichier .env pour les jetons de sécurité.
# Note importante : chown -R 1001:1001 ./data sur l'hôte Proxmox/LXC.
# =========================================================================

services:
  # Interface utilisateur (Frontend)
  rackula:
    image: ghcr.io/rackulalives/rackula:persist
    container_name: rackula
    restart: always
    ports:
      - "8080:8080"
    environment:
      - API_HOST=rackula-api
      - API_PORT=3001
      - RACKULA_LISTEN_PORT=8080
      - API_WRITE_TOKEN=${RACKULA_API_WRITE_TOKEN}
      - RACKULA_AUTH_MODE=none # Accès public (conseillé de filtrer via NPM)
    depends_on:
      rackula-api:
        condition: service_healthy
    networks:
      - rackula-net
    security_opt:
      - no-new-privileges:true
    read_only: true
    tmpfs:
      - /var/cache/nginx:size=10M
      - /var/run:size=1M
      - /tmp:size=5M
      - /etc/nginx/conf.d:size=1M,uid=101,gid=101

  # API de stockage (Backend)
  rackula-api:
    image: ghcr.io/rackulalives/rackula-api:latest
    container_name: rackula-api
    restart: always
    volumes:
      - ./data:/data
    environment:
      - DATA_DIR=/data
      - RACKULA_API_PORT=3001
      - CORS_ORIGIN=${CORS_ORIGIN}
      - RACKULA_API_WRITE_TOKEN=${RACKULA_API_WRITE_TOKEN}
      - RACKULA_AUTH_MODE=none
    networks:
      - rackula-net
    healthcheck:
      test: ["CMD-SHELL", "wget -qO- http://127.0.0.1:3001/health"]
      interval: 30s
      timeout: 10s
    security_opt:
      - no-new-privileges:true
    read_only: true
    tmpfs:
      - /tmp:size=5M

networks:
  rackula-net:
    driver: bridge
```

```plaintext
# =========================================================================
# CONFIGURATION RACKULA (.env)
# Auteur : Amaury aka BlablaLinux (https://link.blablalinux.be)
# =========================================================================

# --- Domaine ---
# URL de votre instance (ex: https://rackula.domaine.be)
CORS_ORIGIN=https://rackula.votre-domaine.be

# --- Sécurité ---
# Générez des clés de 32 ou 64 caractères (openssl rand -hex 32)
RACKULA_API_WRITE_TOKEN=votre_cle_api_secrete_ici
RACKULA_AUTH_SESSION_SECRET=votre_secret_de_session_ici

# --- Mode d'accès ---
# none = libre (protection via NPM conseillée)
# local = avec base de données (non couvert par ce compose)
RACKULA_AUTH_MODE=none
RACKULA_AUTH_SESSION_COOKIE_SECURE=true
RACKULA_AUTH_CSRF_PROTECTION=true
```
