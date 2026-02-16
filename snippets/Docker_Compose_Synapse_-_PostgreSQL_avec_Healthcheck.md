# Docker Compose Synapse - PostgreSQL avec Healthcheck

La particularité de cette stack réside dans l'implémentation d'un Healthcheck robuste sur le service de base de données. Contrairement à un depends_on classique qui vérifie uniquement si le conteneur est démarré, cette configuration utilise service_healthy.

Points clés :

Fiabilité au démarrage : Le service Synapse ne s'initialise que lorsque pg_isready confirme que la base de données accepte les connexions. Cela évite les plantages en boucle du homeserver lors du premier lancement ou des mises à jour.

Optimisation Postgres : Inclusion des arguments d'initialisation (UTF8, collation C) recommandés pour Synapse.

Monitoring : Intégration des healthchecks natifs pour les deux services, facilitant la surveillance de l'état de santé via docker ps ou un dashboard tiers.

• docker
• compose
• yml
• synapse
• postgres
• sysadmin
• selfhosting

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
services:
  synapse:
    image: docker.io/matrixdotorg/synapse:latest
    container_name: synapse
    restart: always
    volumes:
      - ./synapse-data:/data
    depends_on:
      db:
        condition: service_healthy
    ports:
      - 8008:8008
    healthcheck:
      test: ["CMD", "curl", "-fSs", "http://localhost:8008/health"]
      interval: 15s
      timeout: 5s
      retries: 3
      start_period: 5s

  db:
    image: docker.io/postgres:17.5
    container_name: postgres
    restart: always
    environment:
      - POSTGRES_USER=synapse
      - POSTGRES_PASSWORD= # <--- METTRE VOTRE MOT DE PASSE ICI
      - POSTGRES_INITDB_ARGS=--encoding='UTF8' --lc-collate='C' --lc-ctype='C'
    volumes:
      - ./postgres-data:/var/lib/postgresql/data
    healthcheck:
      # On utilise l'utilisateur défini dans POSTGRES_USER
      test: ["CMD-SHELL", "pg_isready -U synapse"]
      interval: 10s
      timeout: 5s
      retries: 5
      start_period: 10s
```
