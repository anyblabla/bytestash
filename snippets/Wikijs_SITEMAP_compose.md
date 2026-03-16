# Wikijs SITEMAP compose

Fichier "docker-compose.yml" à utiliser pour déployer Wiki.js SITEMAP via Docker.

• compose
• yml
• docker
• wiki
• sitemap

```yaml
# =========================================================================
# WIKI.JS SITEMAP GENERATOR (DOCKER COMPOSE)
# Auteur : Amaury aka BlablaLinux (https://link.blablalinux.be)
# -------------------------------------------------------------------------
# Ce service se connecte à la base de données de votre Wiki.js existant 
# pour générer un sitemap automatique.
# 
# Retrouvez la solution complète (Wiki + DB + Sitemap) ici :
# https://bytestash.blablalinux.be/s/6d670a5212ecc6391c9849daa3cdf135
# =========================================================================

services:
  wikijs-sitemap:
    image: hostwiki/wikijs-sitemap:latest
    container_name: sitemaps
    restart: always
    environment:
      # CONFIGURATION DE LA CONNEXION (À calquer sur votre Wiki.js)
      # [IMPORTANT] : Les valeurs ci-dessous doivent correspondre à 
      # votre installation de Wiki.js.
      DB_HOST: 192.168.x.x        # <-- IP du serveur ou nom du service (ex: db)
      DB_NAME: wikidb             # <-- Nom de la base de données
      DB_PASS: strong_pass        # <-- Mot de passe de la base de données
      DB_PORT: 5432
      DB_TYPE: postgres
      DB_USER: wikiuser           # <-- Utilisateur de la base de données
    ports:
      - 3012:3012
    healthcheck:
      test: ["CMD-SHELL", "ps aux | grep node || exit 1"]
      interval: 1m
      timeout: 10s
      retries: 3
```
