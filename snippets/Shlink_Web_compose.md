# Shlink Web compose

Ce déploiement Docker Compose installe le Shlink Web Client, une interface web intuitive pour gérer vos instances Shlink.

Note importante :
Cette interface est purement client-side (elle tourne dans votre navigateur). Elle ne stocke aucune donnée de manière persistante sur le serveur ; elle utilise le LocalStorage de votre navigateur pour mémoriser la connexion à votre serveur Shlink (URL et Clé API).

Configuration :
Les variables d'environnement fournies ici permettent de pré-configurer la connexion à votre moteur Shlink dès le premier chargement de la page.

• docker
• compose
• yml
• shlink
• shorter
• url
• openwebui

```yaml
# =========================================================================
# SHLINK WEB COMPOSE
# Auteur : Amaury aka BlablaLinux (https://link.blablalinux.be)
# =========================================================================

services:
  # --- Shlink Web Client (Interface d'administration) ---
  shlink_web:
    container_name: shlink_web
    image: shlinkio/shlink-web-client:stable
    restart: always
    ports:
      - "8009:8080" # Port 8009 pour l'accès web (à adapter si besoin)
    environment:
      - TZ=Europe/Brussels
      
      # Configuration de l'instance par défaut (Optionnel)
      - SHLINK_SERVER_URL=https://votre-domaine-court.tld
      - SHLINK_SERVER_NAME=Mon-Shortener
      
    healthcheck:
      # Utilisation de l'IP 127.0.0.1 pour éviter les échecs de résolution IPv6 internes
      test: ["CMD", "wget", "--no-verbose", "--tries=1", "--spider", "http://127.0.0.1:8080"]
      interval: 30s
      timeout: 10s
      retries: 3
```
