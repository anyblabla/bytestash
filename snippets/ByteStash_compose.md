# ByteStash compose

Fichier "docker-compose.yml" à utiliser pour déployer ByteStash via Docker.

• docker
• compose
• yml
• bytestash

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
services:
  bytestash:
    image: "ghcr.io/jordan-dalby/bytestash:latest"
    restart: always
    volumes:
      - /your/snippet/path:/data/snippets
    ports:
      - "5000:5000"
    environment:
      # voir https://github.com/jordan-dalby/ByteStash/wiki/FAQ#environment-variables
      BASE_PATH: ""
      JWT_SECRET: your-secret # https://jwtsecret.com/generate
      TOKEN_EXPIRY: 24h
      ALLOW_NEW_ACCOUNTS: "true" # activation inscription
      DEBUG: "true"
      DISABLE_ACCOUNTS: "false"
      DISABLE_INTERNAL_ACCOUNTS: "false"

      # voir https://github.com/jordan-dalby/ByteStash/wiki/Single-Sign-on-Setup pour plus d'infos
      OIDC_ENABLED: "false"
      OIDC_DISPLAY_NAME: ""
      OIDC_ISSUER_URL: ""
      OIDC_CLIENT_ID: ""
      OIDC_CLIENT_SECRET: ""
      OIDC_SCOPES: ""

```
