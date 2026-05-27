# Transmute compose

Ce fichier permet de déployer Transmute via Docker avec ton en-tête PEGAPROX. C'est un outil pratique pour convertir des fichiers. J'ai configuré le volume pour garder tes fichiers en sécurité sur ta machine et mis en place un contrôle de santé pour vérifier que le service tourne bien. N'oublie pas de changer la clé secrète.

• docker
• compose
• yml
• conversion
• file

```yaml
# =========================================================================
# TRANSMUTE COMPOSE
# Auteur : Amaury aka BlablaLinux (https://blablalinux.be/mes-services-publics/)
# =========================================================================

services:
  transmute:
    image: ghcr.io/transmute-app/transmute:latest
    container_name: transmute
    restart: always # Mon restart sera toujours ainsi
    ports:
      - 3313:3313 # Port d'écoute pour le service
    volumes:
      - ./transmute_data:/app/data # Persistance des données localement
    environment:
      # Remplace la clé ci-dessous par une valeur générée aléatoirement pour la sécurité
      - AUTH_SECRET_KEY=TA_CLE_SECRETE_A_CHANGER
      - AUTH_ALGORITHM=HS256
      - AUTH_ACCESS_TOKEN_EXPIRE_MINUTES=120
      - ALLOW_UNAUTHENTICATED=false
      - APP_URL=https://ton-domaine.com
      - PORT=3313
      - HOST=0.0.0.0
      - HOSTS=""
      - CONVERSION_WORKER_CONCURRENCY=5
      - DATA_DIR=data
    healthcheck:
      # Vérification de l'état de santé du conteneur via l'API
      test:
        - "CMD"
        - "wget"
        - "-q"
        - "-O"
        - "/dev/null"
        - "--tries=1"
        - "http://localhost:3313/api/health/ready"
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 40s
```
