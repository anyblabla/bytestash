# LibreTranslate compose

Fichier "docker-compose.yml" à utiliser pour déployer LibreTranslate via Docker.

• yml
• docker
• compose
• translate
• libretranslate

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
services:
  libretranslate:
    image: libretranslate/libretranslate:latest
    container_name: libretranslate
    restart: always
    ports:
      - 5000:5000
    tty: true
    healthcheck:
      test: ['CMD-SHELL', './venv/bin/python scripts/healthcheck.py']
    environment:
      - LT_REQ_LIMIT=30
      - LT_CHAR_LIMIT=1000
      - LT_GA_ID=G-R84Z64ZJX9
      - LT_FRONTEND_TITLE=LibreTranslate BBL
      - LT_GET_API_KEY_LINK=https://blablalinux.be/contact/
      - LT_API_KEYS=true
      - LT_API_KEYS_DB_PATH=/app/db/api_keys.db
      - LT_UPDATE_MODELS=true
      #- LT_LOAD_ONLY=en,fr
    volumes:
      - ./libretranslate_api_keys:/app/db
      - ./libretranslate_models:/home/libretranslate/.local:rw

volumes:
  libretranslate_api_keys:
  libretranslate_models:
```
