# Outline compose

Fichier "docker-compose.yml" à utiliser pour déployer Outline via Docker.

• docker
• compose
• yml
• doc
• markdown
• outline

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
services:
  outline:
    image: docker.getoutline.com/outlinewiki/outline:0.85.1
    container_name: outline
    hostname: outline
    ports:
      - 3000:3000
    volumes:
      - ./storage-data:/var/lib/outline/data
    depends_on:
      - postgres
      - redis
    environment:
      URL: https://outline.blablalinux.be
      PORT: 3000
      WEB_CONCURRENCY: 1
      SECRET_KEY: ca788b2feb2e03f150831803410166b40d4bde997f7ac03a22679164087ad8e3 # pwgen -s 64 1 (commande pour obtenir une clé)
      UTILS_SECRET: e0ba5261ba74ae42c7f363a24d8c1295c821ff4380ac7aa42a23b18741d23669 # pwgen -s 64 1 (commande pour obtenir une clé)
      DEFAULT_LANGUAGE: fr_FR
      DATABASE_URL: postgres://outline:A9b3MKv7457gjF@postgres:5432/outline
      PGSSLMODE: disable
      REDIS_URL: redis://redis:6379
      FILE_STORAGE: local
      FILE_STORAGE_LOCAL_ROOT_DIR: /var/lib/outline/data
      FILE_STORAGE_UPLOAD_MAX_SIZE: 26214400
      #FILE_STORAGE_IMPORT_MAX_SIZE:
      #FILE_STORAGE_WORKSPACE_IMPORT_MAX_SIZE:
      FORCE_HTTPS: true
      SMTP_HOST: smtp.gmail.com
      SMTP_PORT: 587
      SMTP_USERNAME: # nom utilisateur à compléter - exemple: blablalinux@gmail.com
      SMTP_PASSWORD: # mot de passe à compléter
      SMTP_FROM_EMAIL: # exemple: Outline <noreply@blablalinux.be>
      SMTP_TLS_CIPHERS: TLSv1.2
      SMTP_SECURE: false
      RATE_LIMITER_ENABLED: true
      RATE_LIMITER_REQUESTS: 1000
      RATE_LIMITER_DURATION_WINDOW: 60
      ENABLE_UPDATES: true
      DEBUG: http
      LOG_LEVEL: info
    restart: always
  redis:
    container_name: redis
    hostname: redis
    image: redis
    volumes:
      - ./redis.conf:/redis.conf
    command:
      - redis-server
      - /redis.conf
    healthcheck:
      test:
        - CMD
        - redis-cli
        - ping
      interval: 10s
      timeout: 30s
      retries: 3
    restart: always
  postgres:
    image: postgres
    container_name: postgres
    hostname: postgres
    volumes:
      - ./database-data:/var/lib/postgresql/data
    healthcheck:
      test:
        - CMD
        - pg_isready
        - -d
        - outline
        - -U
        - outline
      interval: 30s
      timeout: 20s
      retries: 3
    environment:
      POSTGRES_USER: outline
      POSTGRES_PASSWORD: A9b3MKv7457gjF
      POSTGRES_DB: outline
    restart: always
```
