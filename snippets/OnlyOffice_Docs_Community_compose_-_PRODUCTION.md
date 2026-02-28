# OnlyOffice Docs Community compose - PRODUCTION

Déploiement optimisé en micro-services pour ONLYOFFICE Docs. Cette configuration sépare les composants critiques (PostgreSQL, Redis, RabbitMQ) pour garantir une stabilité maximale, une meilleure gestion des ressources et des mises à jour simplifiées. Elle inclut des sondes de santé (Healthchecks) pour un monitoring précis et une orchestration robuste des conteneurs.

• docker
• compose
• yml
• onlyoffice
• postgresql
• redis
• rabbitmq
• healtcheck

```yaml
# Stack de Production OnlyOffice Docs - Proposée par Amaury aka BlablaLinux : https: //link.blablalinux.be

services:
  onlyoffice-db:
    image: postgres:15-alpine
    container_name: onlyoffice-db
    environment:
      - POSTGRES_DB=onlyoffice
      - POSTGRES_USER=onlyoffice
      - POSTGRES_PASSWORD=onlyoffice
    volumes:
      - ./db:/var/lib/postgresql/data
    ports:
      - 5432:5432 # Exposition pour gestion BDD (Databasus/Datagrip)
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U onlyoffice -d onlyoffice"]
      interval: 10s
      timeout: 5s
      retries: 5
    restart: always

  onlyoffice-rabbitmq:
    image: rabbitmq:3-management-alpine
    container_name: onlyoffice-rabbitmq
    healthcheck:
      test: ["CMD", "rabbitmq-diagnostics", "check_running"]
      interval: 10s
      timeout: 5s
      retries: 5
    restart: always

  onlyoffice-redis:
    image: redis:7-alpine
    container_name: onlyoffice-redis
    healthcheck:
      test: ["CMD", "redis-cli", "ping"]
      interval: 10s
      timeout: 5s
      retries: 5
    restart: always

  documentserver:
    image: onlyoffice/documentserver:latest
    container_name: onlyoffice-server
    depends_on:
      onlyoffice-db:
        condition: service_healthy
      onlyoffice-redis:
        condition: service_healthy
      onlyoffice-rabbitmq:
        condition: service_healthy
    environment:
      - DB_TYPE=postgres
      - DB_HOST=onlyoffice-db
      - DB_PORT=5432
      - DB_NAME=onlyoffice
      - DB_USER=onlyoffice
      - DB_PWD=onlyoffice
      - AMQP_URI=amqp://guest:guest@onlyoffice-rabbitmq
      - REDIS_SERVER_HOST=onlyoffice-redis
      - REDIS_SERVER_PORT=6379
      - JWT_SECRET=VotreSecretIci
      - ALLOW_PRIVATE_IP_ADDRESS=true
    ports:
      - 80:80
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost/healthcheck"]
      interval: 30s
      timeout: 10s
      retries: 3
    volumes:
      - ./data:/var/www/onlyoffice/Data
      - ./logs:/var/log/onlyoffice
      - ./lib:/var/lib/onlyoffice
    restart: always
    tty: true
    stdin_open: true
```
