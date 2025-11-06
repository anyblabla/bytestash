# Kutt compose

Fichier "docker-compose.yml" à utiliser pour déployer Kutt via Docker

• kutt
• docker
• compose
• yml
• short
• url

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
services:
  server:
    image: kutt/kutt:latest
    container_name: kutt
    restart: always
    volumes:
      - ./db_data_sqlite:/var/lib/kutt
      - ./custom:/kutt/custom
    environment:
      - JWT_SECRET=814f9c733663444a53f09faf132145d8dcdd855b
      #- SITE_NAME=Kutt
      - DEFAULT_DOMAIN=
      #- LINK_LENGTH=6
      #- LINK_CUSTOM_ALPHABET=abcdefghj23456789
      #- DISALLOW_REGISTRATION=false
      #- DISALLOW_ANONYMOUS_LINKS=false
      #- TRUST_PROXY=true
      #- DB_CLIENT=better-sqlite3
      - DB_FILENAME=/var/lib/kutt/data.sqlite
      - REDIS_ENABLED=true
      - REDIS_HOST=redis
      - REDIS_PORT=6379
      #- MAIL_ENABLED=true
      #- MAIL_HOST=
      #- MAIL_PORT=587
      #- MAIL_USER=
      #- MAIL_PASSWORD=
      #- MAIL_FROM=
      #- MAIL_SECURE=false
      #- REPORT_EMAIL=
      #- CONTACT_EMAIL=
    ports:
      - 3000:3000
    depends_on:
      redis:
        condition: service_started
  redis:
    image: redis:alpine
    container_name: redis
    restart: always
    expose:
      - 6379
volumes:
  db_data_sqlite: null
  custom: null
```
