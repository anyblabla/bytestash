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
      #- TZ=
      - JWT_SECRET=894f9c733673562a53f09faf131145d8dcdd855b
      #- SITE_NAME=
      - DEFAULT_DOMAIN=
      #- LINK_LENGTH=6
      #- LINK_CUSTOM_ALPHABET=abcdefghjkmnpqrstuvwxyz23456789
      #- DISALLOW_REGISTRATION=false
      #- DISALLOW_ANONYMOUS_LINKS=false
      #- TRUST_PROXY=false
      #- DB_CLIENT=better-sqlite3
      - DB_FILENAME=/var/lib/kutt/data.sqlite
      - REDIS_ENABLED=true
      - REDIS_HOST=redis
      - REDIS_PORT=6379
      #- CUSTOM_DOMAIN_USE_HTTPS=false
      #- ENABLE_RATE_LIMIT=false
      #- MAIL_ENABLED=true
      #- MAIL_HOST=
      #- MAIL_PORT=587
      #- MAIL_USER=
      #- MAIL_PASSWORD=
      #- MAIL_FROM=
      #- MAIL_SECURE=false
      #- REPORT_EMAIL=
      #- CONTACT_EMAIL=
      #- USER_LIMIT_PER_DAY=25
      #- NON_USER_COOLDOWN=5
      #- DEFAULT_MAX_STATS_PER_LINK=1000
      #- RECAPTCHA_SITE_KEY=
      #- RECAPTCHA_SECRET_KEY=
      #- GOOGLE_SAFE_BROWSING_KEY=
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
