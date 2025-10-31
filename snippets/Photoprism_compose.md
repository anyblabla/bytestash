# Photoprism compose

Fichier "docker-compose.yml" à utiliser pour déployer Photoprism via Docker.

• docker
• compose
• yml
• photo

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
services:
  photoprism:
    image: photoprism/photoprism:latest
    container_name: photoprism
    restart: always
    stop_grace_period: 10s
    depends_on:
      - mariadb
    security_opt:
      - seccomp:unconfined
      - apparmor:unconfined
    ports:
      - 2342:2342
    env_file:
      - .env
    working_dir: /photoprism
    volumes:
      - /photoprism/originals:/photoprism/originals
      - /photoprism/storage:/photoprism/storage
  mariadb:
    image: mariadb:11
    container_name: mariadb
    restart: always
    stop_grace_period: 5s
    security_opt:
      - seccomp:unconfined
      - apparmor:unconfined
    command: --innodb-buffer-pool-size=512M --transaction-isolation=READ-COMMITTED
      --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci
      --max-connections=512 --innodb-rollback-on-timeout=OFF
      --innodb-lock-wait-timeout=120
    volumes:
      - /mariadb/database:/var/lib/mysql
    env_file:
      - .env
```

```plaintext
# # Modifications apportées par Blabla Linux : https://link.blablalinux.be
MARIADB_AUTO_UPGRADE=1
MARIADB_INITDB_SKIP_TZINFO=1
MARIADB_DATABASE=photoprism
MARIADB_USER=anyblabla
MARIADB_PASSWORD=blablalinux
MARIADB_ROOT_PASSWORD=BlablaLinux
PHOTOPRISM_DATABASE_DRIVER=mysql
PHOTOPRISM_DATABASE_SERVER=mariadb:3306
PHOTOPRISM_DATABASE_NAME=photoprism
PHOTOPRISM_DATABASE_USER=anyblabla
PHOTOPRISM_DATABASE_PASSWORD=blablalinux
PHOTOPRISM_ADMIN_USER=admin
PHOTOPRISM_ADMIN_PASSWORD=BlablaLinux
PHOTOPRISM_AUTH_MODE=password
PHOTOPRISM_SITE_URL=https://photoprism.blablalinux.be/
PHOTOPRISM_DISABLE_TLS=true
PHOTOPRISM_DEFAULT_TLS=true
PHOTOPRISM_ORIGINALS_LIMIT=5000
PHOTOPRISM_HTTP_COMPRESSION=gzip
PHOTOPRISM_LOG_LEVEL=info
PHOTOPRISM_READONLY=false
PHOTOPRISM_EXPERIMENTAL=false
PHOTOPRISM_DISABLE_CHOWN=false
PHOTOPRISM_DISABLE_WEBDAV=false
PHOTOPRISM_DISABLE_SETTINGS=false
PHOTOPRISM_DISABLE_TENSORFLOW=false
PHOTOPRISM_DISABLE_FACES=false
PHOTOPRISM_DISABLE_CLASSIFICATION=false
PHOTOPRISM_DISABLE_VECTORS=false
PHOTOPRISM_DISABLE_RAW=false
PHOTOPRISM_RAW_PRESETS=false
PHOTOPRISM_JPEG_QUALITY=80
PHOTOPRISM_DETECT_NSFW=false
PHOTOPRISM_UPLOAD_NSFW=true
PHOTOPRISM_SITE_CAPTION=Application de photos alimentée par l'IA
PHOTOPRISM_SITE_DESCRIPTION=
PHOTOPRISM_SITE_AUTHOR=
PHOTOPRISM_FFMPEG_ENCODER=software
PHOTOPRISM_FFMPEG_SIZE=1920
PHOTOPRISM_FFMPEG_BITRATE=32
PHOTOPRISM_INIT=tensorflow
```
