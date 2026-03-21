# Proxcenter Community compose

Déploiement Docker personnalisé de ProxCenter UI optimisé pour un environnement LXC dédié sous Proxmox. Cette configuration utilise le mode réseau bridge (port 3000), un stockage local persistant dans le dossier ./data et inclut un bilan de santé (healthcheck). Le fichier .env est pré-configuré pour une exposition sécurisée derrière un reverse proxy avec support des WebSockets pour les consoles Proxmox.

• docker
• compose
• yml
• proxmox
• dashboard
• center
• proxcenter

```yaml
# =========================================================================
# PROXCENTER UI (DOCKER COMPOSE)
# Auteur : Amaury aka BlablaLinux (https://link.blablalinux.be)
# -------------------------------------------------------------------------

services:
  frontend:
    image: ghcr.io/adminsyspro/proxcenter-frontend:${VERSION:-latest}
    container_name: proxcenter-frontend
    restart: always
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
      - DATABASE_URL=file:/app/data/proxcenter.db
      - APP_SECRET=${APP_SECRET}
      - NEXTAUTH_SECRET=${NEXTAUTH_SECRET}
      - NEXTAUTH_URL=${NEXTAUTH_URL}
      - APP_URL=${APP_URL}
    volumes:
      - ./data:/app/data
    healthcheck:
      test: ["CMD", "wget", "--no-verbose", "--tries=1", "--spider", "http://127.0.0.1:3000/api/health"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 30s
```

```plaintext
# ========================================
# Required - Security Keys
# ========================================
# Generate with: openssl rand -base64 32
APP_SECRET=remplace_moi_par_une_cle_securisee
NEXTAUTH_SECRET=remplace_moi_par_une_autre_cle_securisee

# ========================================
# Required - Application URLs
# ========================================
# Utilise l'IP de ton LXC ou ton domaine (ex: http://192.168.1.50:3000)
NEXTAUTH_URL=http://localhost:3000
APP_URL=http://localhost:3000

# ========================================
# Optional - Version pinning
# ========================================
VERSION=latest
```
