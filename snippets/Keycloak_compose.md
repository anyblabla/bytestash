# Keycloak compose

Fichier "docker-compose.yml" à utiliser pour déployer Keycloak via Docker.

• compose
• docker
• yml
• keycloack

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
services:
  keycloak:
    image: quay.io/keycloak/keycloak:26.1.2
    container_name: keycloak
    restart: always
    command: start-dev
    environment:
      KC_DB: postgres
      KC_DB_URL_HOST: postgres
      KC_DB_URL_DATABASE: keycloak
      KC_DB_USERNAME: keycloak
      KC_DB_PASSWORD: ESnVGsL#9d$8v7
      KC_DB_SCHEMA: public
      KEYCLOAK_ADMIN: admin
      KEYCLOAK_ADMIN_PASSWORD: admin
    ports:
      - "8080:8080"
      - "9000:9000"
    depends_on:
      postgres:
        condition: service_healthy
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:9000/health/live"]
      interval: 10s
      timeout: 10s
      retries: 20

  postgres:
    image: postgres:15
    container_name: postgres
    restart: always
    environment:
      POSTGRES_DB: keycloak
      POSTGRES_USER: keycloak
      POSTGRES_PASSWORD: ESnVGsL#9d$8v7
    volumes:
      - ./postgres_data:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U keycloak"]
      interval: 10s
      timeout: 5s
      retries: 5

volumes:
  postgres_data:
```
