# Isoman compose

ISOMan est un gestionnaire d'images ISO auto-hébergé, performant et moderne. Ce snippet centralise les fichiers Dockerfile, docker-compose.yml et le script d'automatisation pour un déploiement local robuste.

1. PRE-REQUIS

* Git, Docker et le plugin Docker Compose installés.
* Un accès root ou utilisateur dans le groupe docker.

2. PROCEDURE DE DEPLOIEMENT (STEP BY STEP)

* Cloner le dépôt : git clone [https://github.com/aloks98/isoman.git](https://www.google.com/search?q=https://github.com/aloks98/isoman.git) && cd isoman
* Préparer les fichiers : Remplacez le contenu du Dockerfile et du docker-compose.yml par ceux fournis dans ce snippet.
* Lancer le build et le démarrage : docker compose up -d --build
* Vérification : Le service est accessible sur le port 8080. Vérifiez l'état avec "docker ps" (le Healthcheck doit passer au vert après quelques secondes).

3. UTILISATION DU SCRIPT DE MISE A JOUR
Le script update_isoman.sh permet de mettre à jour l'outil sans perdre vos modifications du fichier compose.

* Installation : Copiez le script à la racine de votre dossier utilisateur (ex: /root/update_isoman.sh).
* Permissions : Rendez le script exécutable avec "chmod +x ~/update_isoman.sh".
* Exécution : Lancez simplement ~/update_isoman.sh.
Le script va automatiquement mettre vos réglages en réserve (stash), récupérer les mises à jour GitHub, réappliquer vos réglages, reconstruire l'image et nettoyer les anciens résidus de build.

4. MAINTENANCE
Les données (ISOs et base de données) sont persistantes dans le dossier ./data.

Référence officielle : [https://github.com/aloks98/isoman](https://github.com/aloks98/isoman)

• docker
• compose
• yml
• iso
• download
• file
• disk
• image

```plaintext
# Multi-stage Dockerfile for ISOMan
# Stage 1: Build frontend with Bun
# Stage 2: Build backend with Go
# Stage 3: Create minimal runtime image

# ============================================
# Stage 1: Build Frontend
# ============================================
FROM oven/bun:1-alpine AS frontend-builder

WORKDIR /app/ui

# Copy package files
COPY ui/package.json ui/bun.lock ./

# Install dependencies with cache mount
RUN --mount=type=cache,target=/root/.bun/install/cache \
    bun install --frozen-lockfile

# Copy frontend source
COPY ui/ ./

# Build frontend
RUN bun run build

# ============================================
# Stage 2: Build Backend
# ============================================
FROM golang:1.24-alpine AS backend-builder

# Build argument for version
ARG VERSION=dev

WORKDIR /app

# Install build dependencies
RUN apk add --no-cache git

# Copy go mod files
COPY backend/go.mod backend/go.sum ./

# Download dependencies with cache mount
RUN --mount=type=cache,target=/go/pkg/mod \
    go mod download

# Copy backend source
COPY backend/ ./

# Build backend binary (CGO_ENABLED=0 for static binary)
# Embed version in binary
RUN CGO_ENABLED=0 GOOS=linux go build \
    -ldflags="-w -s -X main.Version=${VERSION}" \
    -o server .

# ============================================
# Stage 3: Runtime Image
# ============================================
FROM alpine:3.19

# Install ca-certificates for HTTPS downloads and su-exec for privilege dropping
RUN apk --no-cache add ca-certificates tzdata su-exec

# Create non-root user
RUN addgroup -g 1000 isoman && \
    adduser -D -u 1000 -G isoman isoman

WORKDIR /app

# Copy backend binary from builder
COPY --from=backend-builder /app/server ./server

# Copy frontend dist from builder
COPY --from=frontend-builder /app/ui/dist ./ui/dist

# Copy migrations directory from builder
COPY --from=backend-builder /app/migrations ./migrations

# Copy entrypoint script
COPY backend/docker-entrypoint.sh /entrypoint.sh

# Create data directory with proper permissions
RUN mkdir -p /data/isos /data/db && \
    chown -R isoman:isoman /app /data && \
    chmod +x /entrypoint.sh

# Set entrypoint (runs as root, then drops to isoman user)
ENTRYPOINT ["/entrypoint.sh"]

# Expose port
EXPOSE 8080

# Environment variables
ENV PORT=8080
ENV DATA_DIR=/data
ENV WORKER_COUNT=2
ENV GIN_MODE=release

# Health check
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD wget --no-verbose --tries=1 --spider http://localhost:8080/health || exit 1

# Run the server (passed to entrypoint)
CMD ["./server"]
```

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
services:
  isoman:
    build: .
    image: isoman:local
    container_name: isoman
    restart: always
    ports:
      - "8080:8080"
    volumes:
      - ./data:/data
    environment:
      - PORT=8080
      - DATA_DIR=/data
      - WORKER_COUNT=2
      - GIN_MODE=release
      - TZ=Europe/Brussels
    healthcheck:
      test: ["CMD", "wget", "--no-verbose", "--tries=1", "--spider", "http://localhost:8080/health"]
      interval: 30s
      timeout: 3s
      start_period: 5s
      retries: 3

```

```bash
#!/bin/bash

# Auteur : Amaury aka BlablaLinux (https://link.blablalinux.be)
# Chemin absolu vers votre installation ISOMan
PROJECT_DIR="/root/isoman"

echo "--- Début de la mise à jour de ISOMan ---"

# Vérifier si le dossier existe
if [ -d "$PROJECT_DIR" ]; then
    cd "$PROJECT_DIR" || exit
    echo "Dossier cible : $(pwd)"
else
    echo "Erreur : Le dossier $PROJECT_DIR n'existe pas."
    exit 1
fi

# 1. Mise en réserve des modifications locales (ton docker-compose.yml)
echo "[1/5] Mise en réserve de vos réglages (Stash)..."
git stash

# 2. Récupération des nouveautés
echo "[2/5] Récupération de la dernière version sur GitHub..."
if git pull origin master; then
    echo "Succès du téléchargement."
else
    echo "Erreur lors du git pull."
    git stash pop
    exit 1
fi

# 3. Réapplication de tes réglages personnels
echo "[3/5] Réapplication de votre docker-compose.yml (Stash pop)..."
git stash pop || echo "Aucune modification locale à réappliquer."

# 4. Relance de Docker (reconstruction avec le Dockerfile local)
echo "[4/5] Reconstruction et redémarrage des conteneurs..."
docker compose up -d --build

# 5. Nettoyage de l'espace disque
echo "[5/5] Nettoyage complet (Images + Builder)..."
docker builder prune -f
docker image prune -f

echo "--- Mise à jour terminée et conteneur opérationnel ! ---"
```
