# SearXNG compose

Fichier "docker-compose.yml" à utiliser pour déployer SearXNG via Docker.

• docker
• compose
• yml
• search
• research

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
services:
  redis:
    container_name: redis
    image: docker.io/valkey/valkey:8-alpine
    command: valkey-server --save 30 1 --loglevel warning
    restart: always
    volumes:
      - ./valkey-data2:/data
    logging:
      driver: json-file
      options:
        max-size: 1m
        max-file: "1"
  searxng:
    container_name: searxng
    image: docker.io/searxng/searxng:2025.7.26-1baf3dc # https://hub.docker.com/r/searxng/searxng/tags
    restart: always
    ports:
      - 0.0.0.0:8080:8080
    volumes:
      - ./searxng:/etc/searxng:rw
      - ./searxng/cache:/var/cache/searxng:rw
    environment:
      - SEARXNG_BASE_URL=https://${SEARXNG_HOSTNAME:-localhost}/
      - UWSGI_WORKERS=${SEARXNG_UWSGI_WORKERS:-4}
      - UWSGI_THREADS=${SEARXNG_UWSGI_THREADS:-4}
    logging:
      driver: json-file
      options:
        max-size: 1m
        max-file: "1"
volumes:
  valkey-data2: null
```

```plaintext
# By default listen on https://localhost
# To change this:
# * uncomment SEARXNG_HOSTNAME, and replace <host> by the SearXNG hostname
# * uncomment LETSENCRYPT_EMAIL, and replace <email> by your email (require to create a Let's Encrypt certificate)

SEARXNG_HOSTNAME=searxng.blablalinux.be
# LETSENCRYPT_EMAIL=<email>

# Optional:
# If you run a very small or a very large instance, you might want to change the amount of used uwsgi workers and threads per worker
# More workers (= processes) means that more search requests can be handled at the same time, but it also causes more resource usage

SEARXNG_UWSGI_WORKERS=4
SEARXNG_UWSGI_THREADS=4
```
