# Memos compose

Fichier "docker-compose.yml" à utiliser pour déployer Memos via Docker.

• docker
• compose
• yml
• memo
• note
• calendar

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
services:
  postgres:
    image: postgres:15
    container_name: postgres
    restart: always
    environment:
      POSTGRES_DB: memos
      POSTGRES_USER: memos
			POSTGRES_PASSWORD: ${DB_PASSWORD}
    volumes:
      - postgres_data:/var/lib/postgresql/data
  memos:
    image: neosmemo/memos:stable
    container_name: memos
    restart: always
    depends_on:
      - postgres
    environment:
			- MEMOS_MODE=prod
      - MEMOS_DRIVER=postgres
      - MEMOS_DSN=postgresql://memos:${DB_PASSWORD}@postgres:5432/memos?sslmode=disable
    ports:
      - 5230:5230
```

```plaintext
# Mot de passe pour la base de données PostgreSQL
DB_PASSWORD=ton_mot_de_passe_securise_ici
```
