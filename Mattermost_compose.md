# Mattermost compose

Fichier "docker-compose.yml" à utiliser pour déployer Mattermost via Docker.

• docker
• compose
• yml
• mattermost

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
# Page de wiki disponible à cette adresse : https://wiki.blablalinux.be/fr/docker-compose-mattermost
# https://docs.docker.com/compose/environment-variables/

services:
  postgres:
    image: postgres:${POSTGRES_IMAGE_TAG}
    container_name: postgres
    restart: ${RESTART_POLICY}
    security_opt:
      - no-new-privileges:true
    pids_limit: 100
    read_only: true
    tmpfs:
      - /tmp
      - /var/run/postgresql
    volumes:
      - ${POSTGRES_DATA_PATH}:/var/lib/postgresql/data
    environment:
      # timezone inside container
      - TZ
      # necessary Postgres options/variables
      - POSTGRES_USER
      - POSTGRES_PASSWORD
      - POSTGRES_DB

  mattermost:
    depends_on:
      - postgres
    image: mattermost/${MATTERMOST_IMAGE}:${MATTERMOST_IMAGE_TAG}
    container_name: mattermost
    ports:
      - ${APP_PORT}:8065
      - ${CALLS_PORT}:${CALLS_PORT}/udp
      - ${CALLS_PORT}:${CALLS_PORT}/tcp
    restart: ${RESTART_POLICY}
    security_opt:
      - no-new-privileges:true
    pids_limit: 200
    read_only: ${MATTERMOST_CONTAINER_READONLY}
    tmpfs:
      - /tmp
    volumes:
      - ${MATTERMOST_CONFIG_PATH}:/mattermost/config:rw
      - ${MATTERMOST_DATA_PATH}:/mattermost/data:rw
      - ${MATTERMOST_LOGS_PATH}:/mattermost/logs:rw
      - ${MATTERMOST_PLUGINS_PATH}:/mattermost/plugins:rw
      - ${MATTERMOST_CLIENT_PLUGINS_PATH}:/mattermost/client/plugins:rw
      - ${MATTERMOST_BLEVE_INDEXES_PATH}:/mattermost/bleve-indexes:rw
      # When you want to use SSO with GitLab, you have to add the cert pki chain of GitLab inside Alpine
      # to avoid Token request failed: certificate signed by unknown authority
      # (link: https://github.com/mattermost/mattermost-server/issues/13059 and https://github.com/mattermost/docker/issues/34)
      # - ${GITLAB_PKI_CHAIN_PATH}:/etc/ssl/certs/pki_chain.pem:ro
    environment:
      # timezone inside container
      - TZ
      # necessary Mattermost options/variables (see env.example)
      - MM_SQLSETTINGS_DRIVERNAME
      - MM_SQLSETTINGS_DATASOURCE
      # necessary for bleve
      - MM_BLEVESETTINGS_INDEXDIR
      # additional settings
      - MM_SERVICESETTINGS_SITEURL

# If you use rolling image tags and feel lucky watchtower can automatically pull new images and
# instantiate containers from it. https://containrrr.dev/watchtower/
# Please keep in mind watchtower will have access on the docker socket. This can be a security risk.
#
#  watchtower:
#    container_name: watchtower
#    image: containrrr/watchtower:latest
#    restart: unless-stopped
#    volumes:
#      - /var/run/docker.sock:/var/run/docker.sock
```

```plaintext
# Domain of service
DOMAIN=mattermost.blablalinux.be

# Container settings
## Timezone inside the containers. The value needs to be in the form 'Europe/Berlin'.
## A list of these tz database names can be looked up at Wikipedia
## https://en.wikipedia.org/wiki/List_of_tz_database_time_zones
TZ=Europe/Brussels
RESTART_POLICY=always

# Postgres settings
## Documentation for this image and available settings can be found on hub.docker.com
## https://hub.docker.com/_/postgres
## Please keep in mind this will create a superuser and it's recommended to use a less privileged
## user to connect to the database.
## A guide on how to change the database user to a nonsuperuser can be found in docs/creation-of-nonsuperuser.md
POSTGRES_IMAGE_TAG=13-alpine
POSTGRES_DATA_PATH=./volumes/db/var/lib/postgresql/data

POSTGRES_USER=blablalinux
POSTGRES_PASSWORD=blablalinux
POSTGRES_DB=mattermost

# Nginx
## The nginx container will use a configuration found at the NGINX_MATTERMOST_CONFIG. The config aims
## to be secure and uses a catch-all server vhost which will work out-of-the-box. For additional settings
## or changes ones can edit it or provide another config. Important note: inside the container, nginx sources
## every config file inside */etc/nginx/conf.d* ending with a *.conf* file extension.

## Inside the container the uid and gid is 101. The folder owner can be set with
## `sudo chown -R 101:101 ./nginx` if needed.
NGINX_IMAGE_TAG=alpine

## The folder containing server blocks and any additional config to nginx.conf
NGINX_CONFIG_PATH=./nginx/conf.d
NGINX_DHPARAMS_FILE=./nginx/dhparams4096.pem

CERT_PATH=./volumes/web/cert/cert.pem
KEY_PATH=./volumes/web/cert/key-no-password.pem
#GITLAB_PKI_CHAIN_PATH=<path_to_your_gitlab_pki>/pki_chain.pem
#CERT_PATH=./certs/etc/letsencrypt/live/${DOMAIN}/fullchain.pem
#KEY_PATH=./certs/etc/letsencrypt/live/${DOMAIN}/privkey.pem

## Exposed ports to the host. Inside the container 80, 443 and 8443 will be used
HTTPS_PORT=443
HTTP_PORT=80
CALLS_PORT=8443

# Mattermost settings
## Inside the container the uid and gid is 2000. The folder owner can be set with
## `sudo chown -R 2000:2000 ./volumes/app/mattermost`.
MATTERMOST_CONFIG_PATH=./volumes/app/mattermost/config
MATTERMOST_DATA_PATH=./volumes/app/mattermost/data
MATTERMOST_LOGS_PATH=./volumes/app/mattermost/logs
MATTERMOST_PLUGINS_PATH=./volumes/app/mattermost/plugins
MATTERMOST_CLIENT_PLUGINS_PATH=./volumes/app/mattermost/client/plugins
MATTERMOST_BLEVE_INDEXES_PATH=./volumes/app/mattermost/bleve-indexes

## Bleve index (inside the container)
MM_BLEVESETTINGS_INDEXDIR=/mattermost/bleve-indexes

## This will be 'mattermost-enterprise-edition' or 'mattermost-team-edition' based on the version of Mattermost you're installing.
MATTERMOST_IMAGE=mattermost-team-edition
## Update the image tag if you want to upgrade your Mattermost version. You may also upgrade to the latest one. The example is based on the latest Mattermost ESR version.
MATTERMOST_IMAGE_TAG=latest

## Make Mattermost container readonly. This interferes with the regeneration of root.html inside the container. Only use
## it if you know what you're doing.
## See https://github.com/mattermost/docker/issues/18
MATTERMOST_CONTAINER_READONLY=false

## The app port is only relevant for using Mattermost without the nginx container as reverse proxy. This is not meant
## to be used with the internal HTTP server exposed but rather in case one wants to host several services on one host
## or for using it behind another existing reverse proxy.
APP_PORT=8065

## Configuration settings for Mattermost. Documentation on the variables and the settings itself can be found at
## https://docs.mattermost.com/administration/config-settings.html
## Keep in mind that variables set here will take precedence over the same setting in config.json. This includes
## the system console as well and settings set with env variables will be greyed out.

## Below one can find necessary settings to spin up the Mattermost container
MM_SQLSETTINGS_DRIVERNAME=postgres
MM_SQLSETTINGS_DATASOURCE=postgres://${POSTGRES_USER}:${POSTGRES_PASSWORD}@postgres:5432/${POSTGRES_DB}?sslmode=disable&connect_timeout=10

## Example settings (any additional setting added here also needs to be introduced in the docker-compose.yml)
MM_SERVICESETTINGS_SITEURL=https://${DOMAIN}
```
