# HedgeDoc compose

Fichier "docker-compose.yml" à utiliser pour déployer HedgeDoc via Docker.

• docker
• compose
• yml
• doc
• markdown

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
services:
  database:
    image: postgres:13.4-alpine
    container_name: postgres
    environment:
      - POSTGRES_USER=hedgedoc
      - POSTGRES_PASSWORD=s7wz8abx32EZP7
      - POSTGRES_DB=hedgedoc
    volumes:
      - ./database:/var/lib/postgresql/data
    restart: always
  app:
    image: quay.io/hedgedoc/hedgedoc:1.10.3
    container_name: hedgedoc
    environment:
      - CMD_DB_URL=postgres://hedgedoc:s7wz4abx32EZP7@database:5432/hedgedoc
      - CMD_DOMAIN=hedgedoc.blablalinux.be
      - CMD_PROTOCOL_USESSL=true
      - CMD_URL_ADDPORT=false
      - CMD_SESSION_SECRET=hi5r8CyMUIycSsNyXnYlNxlAON1xZQBBkqzwiulenNKFDD4Z9hqmPFlmnCQVSIjE
      - HD_AUTH_LOCAL_ENABLE_LOGIN=true
      - HD_AUTH_LOCAL_ENABLE_REGISTER=true
      - NODE_ENV=production
    volumes:
      - ./uploads:/hedgedoc/public/uploads
    ports:
      - 3000:3000
    restart: always
    depends_on:
      - database
volumes:
  database: null
  uploads: null

```
