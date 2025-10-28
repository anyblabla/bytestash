# Wiki.js compose

Fichier "docker-compose.yml" à utiliser pour déployer Wiki.js via Docker.

• compose
• yml
• docker
• wiki

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
# Page de wiki disponible à cette adresse : https://wiki.blablalinux.be/fr/docker-compose-wikijs
services:

  db:
    image: postgres:15-alpine
    container_name: postgres
    environment:
      POSTGRES_DB: wiki
      POSTGRES_PASSWORD: blablalinux
      POSTGRES_USER: anyblabla
    logging:
      driver: "none"
    restart: always
    volumes:
      - ./db-data:/var/lib/postgresql/data

  wiki:
    image: ghcr.io/requarks/wiki:2
    container_name: wiki
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
    volumes:
      - data:/wiki
    ports:
      - "80:3000"

volumes:
  db-data:
  data:
```
