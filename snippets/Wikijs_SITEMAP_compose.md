# Wikijs SITEMAP compose

Fichier "docker-compose.yml" à utiliser pour déployer Wiki.js SITEMAP via Docker.

• compose
• yml
• docker
• wiki
• sitemap

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
# Page de wiki disponible à cette adresse : https://wiki.blablalinux.be/fr/docker-compose-wikijs
services:
  wikijs-sitemap:
    container_name: sitemaps
    image: hostwiki/wikijs-sitemap:latest
    depends_on:
      - db
    environment:
      DB_TYPE: postgres
      DB_HOST: db
      DB_PORT: 5432
      DB_USER: anyblabla
      DB_PASS: blablalinux
      DB_NAME: wiki
    restart: always
    ports:
      - 3012:3012
```
