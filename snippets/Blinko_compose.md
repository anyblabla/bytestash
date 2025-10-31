# Blinko compose

Fichier "docker-compose.yml" à utiliser pour déployer Blinko via Docker.

• docker
• compose
• yml
• blinko

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
services:
  blinko-website:
    image: blinkospace/blinko:1.0.7
    container_name: blinko-website
    environment:
      NODE_ENV: production
      NEXTAUTH_SECRET: CBWxhkhDpTrFpY
      DATABASE_URL: postgresql://postgres:CBWxhkhDpTrFpY@postgres:5432/postgres
    depends_on:
      postgres:
        condition: service_healthy
    volumes:
      - ./app:/app/.blinko
    restart: always
    logging:
      options:
        max-size: 10m
        max-file: "3"
    ports:
      - 1111:1111
    healthcheck:
      test:
        - CMD
        - curl
        - -f
        - http://blinko-website:1111/
      interval: 30s
      timeout: 10s
      retries: 5
      start_period: 30s
    networks:
      - blinko-network
  postgres:
    image: postgres:14
    container_name: blinko-postgres
    restart: always
    ports:
      - 5435:5432
    environment:
      POSTGRES_DB: postgres
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: CBWxhkhDpTrFpY
      TZ: Europe/Brussels
    volumes:
      - ./db:/var/lib/postgresql/data
    healthcheck:
      test:
        - CMD
        - pg_isready
        - -U
        - postgres
        - -d
        - postgres
      interval: 5s
      timeout: 10s
      retries: 5
    networks:
      - blinko-network
networks:
  blinko-network:
    driver: bridge
```
