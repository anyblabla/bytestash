# Nginx Proxy Manager open-appsec (gestion NPM WebUI) compose

Fichier "docker-compose.yml" à utiliser pour déployer Nginx Proxy Manager open-appsec (gestion NPM WebUI) via Docker.

• docker
• compose
• yml
• nginx
• openappsec

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
# Page de wiki disponible à cette adresse : https://wiki.blablalinux.be/fr/docker-compose-nginx-proxy-manager
# Documentation officiel open-appsec : https://docs.openappsec.io/integrations/nginx-proxy-manager/install-nginx-proxy-manager-with-open-appsec-managed-from-npm-webui
services:
  appsec-npm:
    image: ghcr.io/openappsec/nginx-proxy-manager-attachment:latest
    container_name: npm-attachment
    restart: always
    ipc: host
    ports:
      - 80:80
      - 443:443
      - 81:81
    volumes:
      - ./data:/data
      - ./letsencrypt:/etc/letsencrypt
      - ./logrotate.custom:/etc/logrotate.d/nginx-proxy-manager
      - ./appsec-logs:/ext/appsec-logs
      - ./appsec-localconfig:/ext/appsec
    healthcheck:
      test:
        - CMD
        - /usr/bin/check-health
      interval: 10s
      timeout: 3s
  appsec-agent:
    container_name: appsec-agent
    image: ghcr.io/openappsec/agent:latest
    network_mode: service:appsec-npm
    ipc: host
    restart: always
    environment:
      - user_email=<MAIL>
      - nginxproxymanager=true
      - autoPolicyLoad=true
    volumes:
      - ./appsec-config:/etc/cp/conf
      - ./appsec-data:/etc/cp/data
      - ./appsec-logs:/var/log/nano_agent
      - ./appsec-localconfig:/ext/appsec # configuration disponible ici : https://raw.githubusercontent.com/openappsec/open-appsec-npm/main/deployment/local_policy.yaml
      - ./open-appsec-advance-model/open-appsec-advanced-model.tgz:/advanced-model/open-appsec-advanced-model.tgz:rw # fichier disponible sur le cloud NextCloud Blabla Linux : https://nextcloud.blablalinux.be/s/72qwrAqXt3Fzn7F
    command: /cp-nano-agent --standalone

```

```yaml
policies:
  default:
    triggers:
    - appsec-default-log-trigger
    mode: inactive
    practices:
    - webapp-default-practice
    custom-response: appsec-default-web-user-response
  specific-rules: []

practices:
  - name: webapp-default-practice
    web-attacks:
      max-body-size-kb: 1000000
      max-header-size-bytes: 102400
      max-object-depth: 40
      max-url-size-bytes: 32768
      minimum-confidence: high
      override-mode: inactive
      protections:
        csrf-protection: inactive
        error-disclosure: inactive
        non-valid-http-methods: false
        open-redirect: inactive
    anti-bot:
      injected-URIs: []
      validated-URIs: []
      override-mode: inactive
    snort-signatures:
      configmap: []
      override-mode: inactive
    openapi-schema-validation:
      configmap: []
      override-mode: inactive

log-triggers:
  - name: appsec-default-log-trigger
    access-control-logging:
      allow-events: false
      drop-events: true
    additional-suspicious-events-logging:
      enabled: true
      minimum-severity: high
      response-body: false
      response-code: true
    appsec-logging:
      all-web-requests: false
      detect-events: true
      prevent-events: true
    extended-logging:
      http-headers: false
      request-body: false
      url-path: true
      url-query: true
    log-destination:
      cloud: false
      stdout:
        format: json

custom-responses:
  - name: appsec-default-web-user-response
    mode: response-code-only
    http-response-code: 403
```
