# Linkwarden compose

Fichier "docker-compose.yml" à utiliser pour déployer Linkwarden via Docker.

• compose
• docker
• yml
• linkwarden

```yaml
# Modifications apportées par Amaury aka BlablaLinux (https://link.blablalinux.be)
services:
  postgres:
    image: postgres:16-alpine
    container_name: postgres
    env_file: .env
    restart: always
    ports:
      - "5432:5432"
    volumes:
      - "./pgdata:/var/lib/postgresql/data"
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U postgres"]
      interval: 10s
      timeout: 5s
      retries: 5

  linkwarden:
    image: ghcr.io/linkwarden/linkwarden:latest
    container_name: linkwarden
    env_file: .env
    restart: always
    ports:
      - "3000:3000"
    volumes:
      - "./data:/data/data"
    depends_on:
      postgres:
        condition: service_healthy
      meilisearch:
        condition: service_healthy
    healthcheck:
      test: ["CMD-SHELL", "timeout 5s bash -c ':> /dev/tcp/127.0.0.1/3000' || exit 1"]
      interval: 30s
      timeout: 10s
      retries: 3

  meilisearch:
    image: getmeili/meilisearch:v1.12.8
    container_name: meilisearch
    restart: always
    env_file: .env
    environment:
      - MEILI_NO_ANALYTICS=true
    volumes:
      - "./meili_data:/data.ms"
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:7700/health"]
      interval: 10s
      timeout: 5s
      retries: 5
```

```plaintext
# --- CONFIGURATION RÉSEAU ---
# URL de l'API d'authentification (doit finir par /api/v1/auth)
NEXTAUTH_URL=https://votre-domaine.be/api/v1/auth

# Secret pour NextAuth
# Générer avec : openssl rand -base64 32
NEXTAUTH_SECRET=votre_secret_nextauth_ici

# --- BASE DE DONNÉES ---
# Mot de passe pour l'utilisateur postgres
# Générer avec : openssl rand -base64 16
POSTGRES_PASSWORD=votre_mot_de_passe_db_robuste

# URL de connexion à la base de données (utiliser le même mot de passe qu'au-dessus)
DATABASE_URL=postgresql://postgres:votre_mot_de_passe_db_robuste@postgres:5432/postgres

# --- RECHERCHE (MEILISEARCH) ---
MEILI_HOST=http://meilisearch:7700

# Clé Master pour Meilisearch (min. 16 caractères recommandés)
# Générer avec : openssl rand -base64 24
MEILI_MASTER_KEY=votre_cle_meilisearch_tres_longue_ici
```
