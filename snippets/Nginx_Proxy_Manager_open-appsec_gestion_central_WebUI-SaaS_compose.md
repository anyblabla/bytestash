# Nginx Proxy Manager open-appsec (gestion central WebUI-SaaS) compose

Fichier "docker-compose.yml" à utiliser pour déployer Nginx Proxy Manager open-appsec (gestion central WebUI-SaaS) via Docker.

• docker
• compose
• yml
• nginx
• openappsec

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
# Page de wiki disponible à cette adresse : https://wiki.blablalinux.be/fr/docker-compose-nginx-proxy-manager
# Documentation officiel Open AppSec : https://docs.openappsec.io/integrations/nginx-proxy-manager/install-nginx-proxy-manager-with-open-appsec-managed-from-central-webui-saas
services:
  npm-centrally-managed-attachment:
    container_name: npm-centrally-managed-attachment
    image: ghcr.io/openappsec/nginx-proxy-manager-centrally-managed-attachment:latest
    ipc: host
    restart: always
    ports:
      - 80:80
      - 443:443
      - 81:81
    volumes:
      - ./data:/data
      - ./letsencrypt:/etc/letsencrypt
      - ./logrotate.custom:/etc/logrotate.d/nginx-proxy-manager
    healthcheck:
      test:
        - CMD
        - /usr/bin/check-health
      interval: 10s
      timeout: 3s
  appsec-agent:
    container_name: appsec-agent
    image: ghcr.io/openappsec/agent:latest
    ipc: host
    restart: always
    environment:
      - user_email=<MAIL>
      - nginxproxymanager=true
    volumes:
      - ./appsec-config:/etc/cp/conf
      - ./appsec-data:/etc/cp/data
      - ./appsec-logs:/var/log/nano_agent
      - ./open-appsec-advance-model/open-appsec-advanced-model.tgz:/advanced-model/open-appsec-advanced-model.tgz:rw # fichier disponible sur le cloud NextCloud Blabla Linux (https://nextcloud.blablalinux.be/s/72qwrAqXt3Fzn7F) ou se votre compte (https://my.openappsec.io)
    command: /cp-nano-agent --token <TOKEN>

```
