# Listmonk compose

Ce déploiement Docker Compose propose une instance Listmonk prête pour la production, couplée à une base de données PostgreSQL 17 (Alpine). La configuration inclut des healthchecks pour garantir que l'application ne démarre qu'une fois la base prête, ainsi qu'un conteneur Watchtower pour la mise à jour automatique des images. Idéal pour gérer ses newsletters avec une approche respectueuse de la vie privée.

• docker
• compose
• yml
• self-hosted
• mailing
• postgresql
• listmonk
• newsletter

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
services:
  # Base de données PostgreSQL (Version 17 légère)
  listmonk_db:
    image: postgres:17-alpine
    container_name: listmonk_db
    restart: unless-stopped
    environment:
      - POSTGRES_USER=listmonk        # Utilisateur de la base
      - POSTGRES_PASSWORD=votre_mdp    # REMPLACER PAR UN MOT DE PASSE FORT
      - POSTGRES_DB=listmonk          # Nom de la base de données
    volumes:
      - ./db:/var/lib/postgresql/data # Persistance des données (relatif)
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U listmonk -d listmonk"]
      interval: 10s
      timeout: 5s
      retries: 5

  # Application Listmonk
  listmonk_app:
    image: listmonk/listmonk:latest
    container_name: listmonk_app
    restart: unless-stopped
    depends_on:
      listmonk_db:
        condition: service_healthy    # Attendre que la DB soit prête
    ports:
      - "9000:9000"                   # Port d'accès à l'interface
    environment:
      - LISTMONK_db__host=listmonk_db
      - LISTMONK_db__user=listmonk
      - LISTMONK_db__password=votre_mdp # DOIT CORRESPONDRE AU MOT DE PASSE CI-DESSUS
      - LISTMONK_db__database=listmonk
      - LISTMONK_db__port=5432
      - LISTMONK_app__address=0.0.0.0:9000
    volumes:
      - ./config.toml:/listmonk/config.toml # Fichier de config principal
      - ./uploads:/listmonk/uploads         # Dossier pour logos, favicons et médias
    healthcheck:
      test: ["CMD", "wget", "--quiet", "--tries=1", "--spider", "http://localhost:9000/healthlib"]
      interval: 30s
      timeout: 10s
      retries: 3
```
