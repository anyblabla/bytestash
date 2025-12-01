# NPM - Open Appsec SaaS - Fail2Ban - GoAccess

Fichier "docker-compose.yml" à utiliser pour déployer l'enble NPM, Open Appsec, Fail2Ban et GoAcces via Docker.

• compose
• docker
• yml
• npm
• appsec
• fail2ban
• goaccess

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
services:
  appsec-npm:
    container_name: npm-attachment
    image: 'ghcr.io/openappsec/nginx-proxy-manager-centrally-managed-attachment:latest'
    ipc: host
    restart: always
    ports:
      - '80:80'
      - '443:443'
      - '81:81'
      #- '21:21'
    environment:
      - X_FRAME_OPTIONS="sameorigin"
    volumes:
      - ./npm-data:/data
      - ./npm-letsencrypt:/etc/letsencrypt
      - ./npm-logrotate.custom:/etc/logrotate.d/nginx-proxy-manager
    healthcheck:
      test: ["CMD", "/usr/bin/check-health"]
      interval: 10s
      timeout: 3s

  appsec-agent:
    container_name: appsec-agent
    image: 'ghcr.io/openappsec/agent:latest'
    ipc: host
    restart: always
    environment:
      - user_email=amaury-libert@hotmail.com
      - nginxproxymanager=true
    volumes:
      - ./appsec-config:/etc/cp/conf
      - ./appsec-data:/etc/cp/data
      - ./appsec-logs:/var/log/nano_agent
      - ./open-appsec-advance-model/open-appsec-advanced-model.tgz:/advanced-model/open-appsec-advanced-model.tgz:rw # fichier disponible sur le cloud NextCloud Blabla Linux (https://nextcloud.blablalinux.be/s/72qwrAqXt3Fzn7F) ou se votre compte (https://my.openappsec.io)
    command: /cp-nano-agent --token <TOKEN>

  fail2ban:
    image: crazymax/fail2ban:latest
    container_name: fail2ban
    network_mode: host
    cap_add:
      - NET_ADMIN
      - NET_RAW
    volumes:
      - ./fail2ban-data:/data
      - ./npm-data/logs:/var/log:ro
    environment:
      - TZ=Europe/Brussels
      - F2B_LOG_TARGET=STDOUT
      - F2B_LOG_LEVEL=INFO
      - F2B_DB_PURGE_AGE=28d
    restart: always

  goaccess:
    image: 'xavierh/goaccess-for-nginxproxymanager:latest'
    container_name: goaccess
    restart: always
    ports:
      - '7880:7880'
    environment:
      - TZ=Europe/Brussels
      - LANG=fr_FR.UTF-8
      - LANGUAGE=fr_FR.UTF-8
      - SKIP_ARCHIVED_LOGS=False
      - DEBUG=False
      - BASIC_AUTH=False
      - BASIC_AUTH_USERNAME=user
      - BASIC_AUTH_PASSWORD=pass
      - EXCLUDE_IPS=127.0.0.1
      - LOG_TYPE=NPM+R
      - ENABLE_BROWSERS_LIST=True
      - CUSTOM_BROWSERS=Kuma:Uptime,TestBrowser:Crawler
      - HTML_REFRESH=5
      - KEEP_LAST=30
      - PROCESSING_THREADS=1
    volumes:
      - ./npm-data/logs:/opt/log
```
