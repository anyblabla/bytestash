# MiniQR compose

Instance de MiniQR, une application légère et moderne pour générer des codes QR personnalisés. Cette version est optimisée pour fonctionner derrière Nginx Proxy Manager (NPM) sans proxy interne superflu.

• docker
• compose
• yml
• qr code

```yaml
# =========================================================================
# MINIQR COMPOSE
# Auteur : Amaury aka BlablaLinux (https://link.blablalinux.be)
# =========================================================================

services:
  mini-qr:
    image: ghcr.io/lyqht/mini-qr:latest
    container_name: mini-qr
    restart: always
    ports:
      - 8080:8080
    networks:
      - mini-qr-network
    healthcheck:
      test: ["CMD", "wget", "--no-verbose", "--tries=1", "--spider", "http://localhost:8080/"]
      interval: 30s
      timeout: 10s
      retries: 3

networks:
  mini-qr-network:
    driver: bridge
```
