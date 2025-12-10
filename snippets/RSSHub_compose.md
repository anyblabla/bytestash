# RSSHub compose

Fichier "docker-compose.yml" à utiliser pour déployer RSSHub via Docker.
Page sur le wiki : https://wiki.blablalinux.be/fr/rsshub-flux-universel

• compose
• docker
• yml
• rss
• hub

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
# Page sur le wiki : https://wiki.blablalinux.be/fr/rsshub-flux-universel
services:
    rsshub:
        # two ways to enable puppeteer:
        # * comment out marked lines, then use this image instead: diygod/rsshub:chromium-bundled
        # * (consumes more disk space and memory) leave everything unchanged
        image: diygod/rsshub:latest # or ghcr.io/diygod/rsshub
        container_name: rsshub
        restart: always
        ports:
            - '1200:1200'
        environment:
            NODE_ENV: production
            CACHE_TYPE: redis
            REDIS_URL: 'redis://redis:6379/'
            PUPPETEER_WS_ENDPOINT: 'ws://browserless:3000' # marked
            PUPPETEER_REAL_BROWSER_SERVICE: 'http://real-browser:3000' # marked
            ALLOW_ORIGIN: '*'
        healthcheck:
            test: ['CMD', 'curl', '-f', 'http://localhost:1200/healthz']
            interval: 30s
            timeout: 10s
            retries: 3
        depends_on:
            - redis
            - browserless # marked
    real-browser:
        image: ghcr.io/hyoban/puppeteer-real-browser-hono:latest
        container_name: real-browser
        restart: always
        ports:
            - '3001:3000'
        healthcheck:
            test: ['CMD', 'curl', '-f', 'http://localhost:3000']
            interval: 30s
            timeout: 10s
            retries: 3
    browserless: # marked
        image: browserless/chrome:latest # marked
        container_name: browserless
        restart: always # marked
        ulimits: # marked
            core: # marked
                hard: 0 # marked
                soft: 0 # marked
        healthcheck: # marked
            test: ['CMD', 'curl', '-f', 'http://localhost:3000/pressure'] # marked
            interval: 30s # marked
            timeout: 10s # marked
            retries: 3 # marked
    redis:
        image: redis:alpine
        container_name: redis
        restart: always
        volumes:
            - ./redis-data:/data
        healthcheck:
            test: ['CMD', 'redis-cli', 'ping']
            interval: 30s
            timeout: 10s
            retries: 5
            start_period: 5s
```
