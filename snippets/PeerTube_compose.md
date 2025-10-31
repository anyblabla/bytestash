# PeerTube compose

Fichier "docker-compose.yml" à utiliser pour déployer PeerTube via Docker.

• docker
• compose
• yml
• video
• peertube

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
services:
  webserver:
    image: chocobozzz/peertube-webserver:latest
    container_name: peertube-webserver
    env_file:
      - .env
    ports:
      - 80:80
      - 443:443
    volumes:
      - ./docker-volume/nginx/peertube:/etc/nginx/conf.d/peertube.template
      - assets:/var/www/peertube/peertube-latest/client/dist:ro
      - ./docker-volume/data:/var/www/peertube/storage
      - ./docker-volume/nginx-logs:/var/log/nginx
    depends_on:
      - peertube
    restart: always
  peertube:
    image: chocobozzz/peertube:v7.2.3-bookworm # https://github.com/Chocobozzz/PeerTube/releases
    container_name: peertube
    networks:
      default:
        ipv4_address: 172.18.0.42
        ipv6_address: fdab:e4b3:21a2:ef1b::42
    env_file:
      - .env
    ports:
      - 1935:1935
      - 9000:9000
    volumes:
      - assets:/app/client/dist
      - ./docker-volume/data:/data
      - ./docker-volume/config:/config
    depends_on:
      - postgres
      - redis
      - postfix
    restart: always
  postgres:
    image: postgres:13-alpine
    container_name: peertube-postgres
    env_file:
      - .env
    volumes:
      - ./docker-volume/db:/var/lib/postgresql/data
    restart: always
  redis:
    image: redis:6-alpine
    container_name: peertube-redis
    volumes:
      - ./docker-volume/redis:/data
    restart: always
  postfix:
    image: mwader/postfix-relay
    container_name: peertube-postfix
    env_file:
      - .env
    volumes:
      - ./docker-volume/opendkim/keys:/etc/opendkim/keys
    restart: always
networks:
  default:
    enable_ipv6: true
    ipam:
      driver: default
      config:
        - subnet: 172.18.0.0/16
        - subnet: fdab:e4b3:21a2:ef1b::/64
volumes:
  assets: null

```

```plaintext
POSTGRES_USER=anyblabla
POSTGRES_PASSWORD=blablalinux
POSTGRES_DB=peertube
PEERTUBE_DB_NAME=peertube
PEERTUBE_DB_SUFFIX=_prod
PEERTUBE_DB_USERNAME=anyblabla
PEERTUBE_DB_PASSWORD=blablalinux
PEERTUBE_DB_SSL=false
PEERTUBE_DB_HOSTNAME=postgres
PEERTUBE_WEBSERVER_HOSTNAME=peertube.blablalinux.be
PEERTUBE_WEBSERVER_PORT=443
PEERTUBE_WEBSERVER_HTTPS=true
PEERTUBE_TRUST_PROXY=["0.0.0.0", "127.0.0.1", "loopback", "172.18.0.0/16", "192.168.2.210"]
PEERTUBE_SECRET=4dffccab6c512438abfc7f42ae70bf57c7ce29fa399d0f43b6a8d24f4a4e93e5 # openssl rand -hex 32
PEERTUBE_SMTP_USERNAME=
PEERTUBE_SMTP_PASSWORD=
PEERTUBE_SMTP_HOSTNAME=smtp.gmail.com
PEERTUBE_SMTP_PORT=465
PEERTUBE_SMTP_FROM=PeerTube <notifications@peertube.blablalinux.be>
PEERTUBE_SMTP_TLS=true
PEERTUBE_SMTP_DISABLE_STARTTLS=false
PEERTUBE_ADMIN_EMAIL=
#PEERTUBE_LOG_LEVEL=info
#PEERTUBE_SIGNUP_ENABLED=false
#PEERTUBE_TRANSCODING_ENABLED=true
#PEERTUBE_CONTACT_FORM_ENABLED=true
```
