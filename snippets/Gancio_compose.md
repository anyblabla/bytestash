# Gancio compose

Fichier "docker-compose.yml" à utiliser pour déployer Gancio via Docker.

• docker
• compose
• yml
• gancio
• calendar

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
services:
  gancio:
    restart: always
    image: cisti/gancio:latest
    container_name: gancio
    environment:
      - PATH=$PATH:/home/node/.yarn/bin
      - GANCIO_DATA=/home/node/data
      - NODE_ENV=production
      - GANCIO_DB_DIALECT=sqlite
      - GANCIO_DB_STORAGE=./gancio.sqlite
    volumes:
      - ./data:/home/node/data
    ports:
      - "0.0.0.0:13120:13120"
```
