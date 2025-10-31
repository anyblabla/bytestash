# Mastodon compose

Fichier "docker-compose.yml" à utiliser pour déployer Mastodon via Docker.

• docker
• compose
• yml
• mastodon

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
# This file is designed for production server deployment, not local development work
# For a containerized local dev environment, see: https://github.com/mastodon/mastodon/blob/main/README.md#docker
# Fichier "docker-compose.yml" disponible ici : https://github.com/mastodon/mastodon/blob/main/docker-compose.yml
services:
  db:
    restart: always
    image: postgres:14-alpine
    container_name: postgres
    shm_size: 256mb
    networks:
      - internal_network
    healthcheck:
      test:
        - CMD
        - pg_isready
        - -U
        - postgres
    volumes:
      - ./postgres14:/var/lib/postgresql/data
    environment:
      - POSTGRES_HOST_AUTH_METHOD=trust
  redis:
    restart: always
    image: redis:7-alpine
    container_name: redis
    networks:
      - internal_network
    healthcheck:
      test:
        - CMD
        - redis-cli
        - ping
    volumes:
      - ./redis:/data
  es:
    restart: always
    image: docker.elastic.co/elasticsearch/elasticsearch:7.17.4
    container_name: elasticsearch
    environment:
      - ES_JAVA_OPTS=-Xms512m -Xmx512m -Des.enforce.bootstrap.checks=true
      - xpack.license.self_generated.type=basic
      - xpack.security.enabled=false
      - xpack.watcher.enabled=false
      - xpack.graph.enabled=false
      - xpack.ml.enabled=false
      - cluster.name=es-mastodon
      - discovery.type=single-node
      - thread_pool.write.queue_size=1000
    networks:
      - external_network
      - internal_network
    healthcheck:
      test:
        - CMD-SHELL
        - curl --silent --fail localhost:9200/_cluster/health || exit 1
    volumes:
      - ./elasticsearch:/usr/share/elasticsearch/data
    ports:
      - 0.0.0.0:9200:9200
  web:
    image: ghcr.io/mastodon/mastodon:v4.4.1 # https://github.com/mastodon/mastodon/tags
    container_name: mastodon-web
    restart: always
    env_file: .env.production
    command: bundle exec puma -C config/puma.rb
    networks:
      - external_network
      - internal_network
    healthcheck:
      test:
        - CMD-SHELL
        - curl -s --noproxy localhost localhost:3000/health | grep -q 'OK' ||
          exit 1
    ports:
      - 0.0.0.0:3000:3000
    depends_on:
      - db
      - redis
      - es
    volumes:
      - ./public/system:/mastodon/public/system
  streaming:
    image: ghcr.io/mastodon/mastodon-streaming:v4.4.1 # https://github.com/mastodon/mastodon/tags
    container_name: mastodon-streaming
    restart: always
    env_file: .env.production
    command: node ./streaming/index.js
    networks:
      - external_network
      - internal_network
    healthcheck:
      test:
        - CMD-SHELL
        - curl -s --noproxy localhost localhost:4000/api/v1/streaming/health |
          grep -q 'OK' || exit 1
    ports:
      - 0.0.0.0:4000:4000
    depends_on:
      - db
      - redis
  sidekiq:
    image: ghcr.io/mastodon/mastodon:v4.4.1 # https://github.com/mastodon/mastodon/tags
    container_name: mastodon-sidekiq
    restart: always
    env_file: .env.production
    command: bundle exec sidekiq
    depends_on:
      - db
      - redis
    networks:
      - external_network
      - internal_network
    volumes:
      - ./public/system:/mastodon/public/system
    healthcheck:
      test:
        - CMD-SHELL
        - ps aux | grep '[s]idekiq 7' || false
networks:
  external_network: null
  internal_network:
    internal: true
```

```plaintext
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
# Fichier ".env.production" disponible ici : https://github.com/mastodon/mastodon/blob/main/.env.production.sample
LOCAL_DOMAIN=
SINGLE_USER_MODE=false
SECRET_KEY_BASE=
OTP_SECRET=
ACTIVE_RECORD_ENCRYPTION_DETERMINISTIC_KEY=
ACTIVE_RECORD_ENCRYPTION_KEY_DERIVATION_SALT=
ACTIVE_RECORD_ENCRYPTION_PRIMARY_KEY=
VAPID_PRIVATE_KEY=
VAPID_PUBLIC_KEY=
DB_HOST=db
DB_PORT=5432
DB_NAME=mastodon
DB_USER=mastodon
DB_PASS=
REDIS_HOST=redis
REDIS_PORT=6379
REDIS_PASSWORD=
SMTP_SERVER=smtp.gmail.com
SMTP_PORT=587
SMTP_LOGIN=
SMTP_PASSWORD=
SMTP_AUTH_METHOD=plain
SMTP_OPENSSL_VERIFY_MODE=none
SMTP_ENABLE_STARTTLS=auto
SMTP_FROM_ADDRESS=
HCAPTCHA_SITE_KEY=
HCAPTCHA_SECRET_KEY=
DEEPL_API_KEY=
DEEPL_PLAN=free
ES_ENABLED=true
ES_HOST=es
ES_PORT=9200
ES_PRESET=single_node_cluster
#ES_USER=
#ES_PASS=
```
