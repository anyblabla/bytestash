# LibreTranslate compose

Fichier "docker-compose.yml" à utiliser pour déployer LibreTranslate via Docker.

• yml
• docker
• compose
• translate
• libretranslate

```yaml
services:
  libretranslate:
    image: libretranslate/libretranslate:latest
    container_name: libretranslate
    restart: always
    ports:
      - 5000:5000
    tty: true
    healthcheck:
      test:
        - CMD-SHELL
        - ./venv/bin/python scripts/healthcheck.py
    environment:
      - LT_API_KEYS=true
	    - LT_API_KEYS_DB_PATH=/app/db/api_keys.db
	    #- LT_API_KEYS_REMOTE
	    #- LT_BATCH_LIMIT
	    - LT_CHAR_LIMIT=1000
	    #- LT_DEBUG
	    #- LT_DISABLE_FILES_TRANSLATION
	    #- LT_DISABLE_WEB_UI
	    #- LT_FRONTEND_LANGUAGE_SOURCE
      #- LT_FRONTEND_LANGUAGE_TARGET
      #- LT_FRONTEND_TIMEOUT
      - LT_FRONTEND_TITLE=
      - LT_GA_ID=
      - LT_GET_API_KEY_LINK=
      #- LT_HOST
      #- LT_LOAD_ONLY=en,fr
      #- LT_METRICS
      #- LT_METRICS_AUTH_TOKEN
      #- LT_PORT
      - LT_REQ_LIMIT=30
      #- LT_REQ_LIMIT_STORAGE
      #- LT_REQUIRE_API_KEY_ORIGIN
      #- LT_REQUIRE_API_KEY_SECRET
      #- LT_SHARED_STORAGE
      #- LT_SSL
      #- LT_SUGGESTIONS
      - LT_THREADS=4
      - LT_UPDATE_MODELS=true
      #- LT_URL_PREFIX
    volumes:
      - ./libretranslate_api_keys:/app/db
      - ./libretranslate_models:/home/libretranslate/.local:rw
volumes:
  libretranslate_api_keys: null
  libretranslate_models: null
```