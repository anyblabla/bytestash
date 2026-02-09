# 2FAuth compose - Standard vs Performance avec Redis

Deux variantes de déploiement pour 2FAuth. La version "Standard" est idéale pour la simplicité (tout-en-un avec SQLite), tandis que la version "Performance" utilise Redis pour décharger les entrées/sorties disque (E/S) et accélérer la gestion des sessions et du cache.

• docker
• compose
• yml
• 2fauth
• redis
• password

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
services:
  2fauth:
    image: 2fauth/2fauth:latest
    restart: always
    container_name: 2fauth
    volumes:
      - ./data:/2fauth
    ports:
      - 8000:8000/tcp
    environment:
      - APP_NAME=2FAuth
      - APP_ENV=local
      - APP_TIMEZONE=Europe/Brussels
      - APP_DEBUG=false
      - SITE_OWNER=admin@example.com
      - APP_KEY=base64:GENERATE_YOUR_OWN_KEY_HERE=
      - APP_URL=https://2fauth.your-domain.com
      - ASSET_URL=https://2fauth.your-domain.com
      - IS_DEMO_APP=false
      - LOG_CHANNEL=daily
      - LOG_LEVEL=notice
      - DB_DATABASE="/2fauth/database.sqlite"

      # Drivers en mode local (sans Redis)
      - CACHE_DRIVER=file
      - SESSION_DRIVER=file
      - QUEUE_DRIVER=sync

      # Mail settings
      - MAIL_MAILER=smtp
      - MAIL_HOST=smtp.your-provider.com
      - MAIL_PORT=587
      - MAIL_USERNAME=your-email@example.com
      - MAIL_PASSWORD=your-app-password
      - MAIL_ENCRYPTION=tls
      - MAIL_FROM_NAME=2Fauth
      - MAIL_FROM_ADDRESS=your-email@example.com
      - MAIL_VERIFY_SSL_PEER=true

      # Security & Auth
      - THROTTLE_API=60
      - LOGIN_THROTTLE=5
      - AUTHENTICATION_GUARD=web-guard
      - AUTHENTICATION_LOG_RETENTION=365
      - WEBAUTHN_NAME=2FAuth
      - WEBAUTHN_ID=your-domain.com
      - WEBAUTHN_USER_VERIFICATION=preferred
      - TRUSTED_PROXIES=*

      - BROADCAST_DRIVER=log
      - SESSION_LIFETIME=120
      - MIX_ENV=local
    healthcheck:
      test: ["CMD-SHELL", "wget -q --spider http://localhost:8000/login || exit 1"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 30s
```

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
services:
  2fauth:
    image: 2fauth/2fauth:latest
    restart: always
    container_name: 2fauth
    volumes:
      - ./data:/2fauth
    ports:
      - 8000:8000/tcp
    environment:
      - APP_NAME=2FAuth
      - APP_ENV=local
      - APP_TIMEZONE=Europe/Brussels
      - APP_DEBUG=false
      - SITE_OWNER=admin@example.com
      - APP_KEY=base64:GENERATE_YOUR_OWN_KEY_HERE= # Utiliser 'php artisan key:generate'
      - APP_URL=https://2fauth.your-domain.com
      - ASSET_URL=https://2fauth.your-domain.com
      - IS_DEMO_APP=false
      - LOG_CHANNEL=daily
      - LOG_LEVEL=notice
      - DB_DATABASE="/2fauth/database.sqlite"

      # Caching & Sessions
      - CACHE_DRIVER=redis
      - SESSION_DRIVER=redis
      - QUEUE_DRIVER=redis
      - REDIS_HOST=redis
      - REDIS_PASSWORD=null
      - REDIS_PORT=6379

      # Mail settings (SMTP Example)
      - MAIL_MAILER=smtp
      - MAIL_HOST=smtp.your-provider.com
      - MAIL_PORT=587
      - MAIL_USERNAME=your-email@example.com
      - MAIL_PASSWORD=your-app-password
      - MAIL_ENCRYPTION=tls
      - MAIL_FROM_NAME=2Fauth
      - MAIL_FROM_ADDRESS=your-email@example.com
      - MAIL_VERIFY_SSL_PEER=true

      # Security & Auth
      - THROTTLE_API=60
      - LOGIN_THROTTLE=5
      - AUTHENTICATION_GUARD=web-guard
      - AUTHENTICATION_LOG_RETENTION=365
      - WEBAUTHN_NAME=2FAuth
      - WEBAUTHN_ID=your-domain.com
      - WEBAUTHN_USER_VERIFICATION=preferred
      - TRUSTED_PROXIES=*

      - BROADCAST_DRIVER=log
      - SESSION_LIFETIME=120
      - MIX_ENV=local
    healthcheck:
      test: ["CMD-SHELL", "wget -q --spider http://localhost:8000/login || exit 1"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 30s
    depends_on:
      redis:
        condition: service_healthy

  redis:
    image: redis:alpine
    container_name: 2fauth_redis
    restart: always
    healthcheck:
      test: ["CMD", "redis-cli", "ping"]
      interval: 10s
      timeout: 5s
      retries: 5
```
