# Gitea compose

Fichier "docker-compose.yml" à utiliser pour déployer Gitea via Docker.

• docker
• compose
• yml
• git
• code

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
services:
  server:
    image: docker.gitea.com/gitea:1.24.6
    container_name: gitea
    environment:
      - USER_UID=1000
      - USER_GID=1000
      - GITEA__database__DB_TYPE=postgres
      - GITEA__database__HOST=db:5432
      - GITEA__database__NAME=gitea
      - GITEA__database__USER=gitea
      - GITEA__database__PASSWD=XWdfnb4eMsttML
    restart: always
    networks:
      - gitea
    volumes:
      - ./gitea:/data
      - /etc/timezone:/etc/timezone:ro
      - /etc/localtime:/etc/localtime:ro
    ports:
      - 3000:3000
      - 222:22
    depends_on:
      - db
  db:
    image: docker.io/library/postgres:14
    container_name: postgres
    environment:
      - POSTGRES_USER=gitea
      - POSTGRES_PASSWORD=XWdfnb4eMsttML
      - POSTGRES_DB=gitea
    restart: always
    networks:
      - gitea
    volumes:
      - ./postgres:/var/lib/postgresql/data
networks:
  gitea:
    external: false
```
