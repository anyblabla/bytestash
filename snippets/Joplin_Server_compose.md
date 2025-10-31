# Joplin Server compose

Fichier "docker-compose.yml" à utiliser pour déployer Joplin Server via Docker.

• docker
• compose
• yml
• joplin
• server
• notes

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
services:
  db:
    image: postgres:16
    volumes:
      - ./postgres:/var/lib/postgresql/data
    ports:
      - 5432:5432
    restart: always
    container_name: postgres
    environment:
      - POSTGRES_PASSWORD=F2k8Sywvvn2eFi
      - POSTGRES_USER=joplin
      - POSTGRES_DB=joplindb
  app:
    image: joplin/server:latest
    depends_on:
      - db
    ports:
      - 22300:22300
    restart: always
    container_name: joplin
    environment:
      - APP_PORT=22300
      - APP_BASE_URL=ip-address_or_domain-name  # à modifier
      - DB_CLIENT=pg
      - POSTGRES_PASSWORD=F2k8Sywvvn2eFi
      - POSTGRES_DATABASE=joplindb
      - POSTGRES_USER=joplin
      - POSTGRES_PORT=5432
      - POSTGRES_HOST=db
      - MAILER_ENABLED=1  # 0 désactive l'option de création de compte sur la page de connexion
      - MAILER_HOST=smtp.gmail.com
      - MAILER_PORT=587
      - MAILER_SECURITY=starttls
      - MAILER_AUTH_USER=email_address  # à modifier
      - MAILER_AUTH_PASSWORD=password  # à modifier
      - MAILER_NOREPLY_NAME=JoplinServer
      - MAILER_NOREPLY_EMAIL=email_address  # à modifier
```
