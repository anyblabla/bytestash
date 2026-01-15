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
    container_name: 2fauth
    restart: always
    volumes:
      - ./data:/2fauth
    ports:
      - 8000:8000/tcp
    environment:
      - APP_NAME=2FAuth
      - APP_ENV=local
      - APP_TIMEZONE=Europe/Brussels
      - APP_DEBUG=false
      - SITE_OWNER=votre-email@exemple.com
      - APP_KEY=base64:GENEREZ_VOTRE_CLE_ICI_32_CHARS # Utilisez 'php artisan key:generate'
      - APP_URL=https://votre-url.be
      - ASSET_URL=https://votre-url.be
      
      # Configuration de la base de données (SQLite par défaut)
      - DB_DATABASE="/srv/database/database.sqlite"
      
      # Drivers en mode fichier (pas de container additionnel requis)
      - CACHE_DRIVER=file
      - SESSION_DRIVER=file
      - QUEUE_DRIVER=sync
      
      # Configuration Mail (SMTP)
      - MAIL_MAILER=smtp
      - MAIL_HOST=votre.serveur-smtp.com
      - MAIL_PORT=587
      - MAIL_USERNAME=votre-login
      - MAIL_PASSWORD=votre-mot-de-passe
      - MAIL_ENCRYPTION=tls
      - MAIL_FROM_NAME=2Fauth
      - MAIL_FROM_ADDRESS=votre-email@exemple.com
```

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
services:
  2fauth:
    image: 2fauth/2fauth:latest
    container_name: 2fauth
    restart: always
    depends_on:
      - redis
    volumes:
      - ./data:/2fauth
    ports:
      - 8000:8000/tcp
    environment:
      - APP_NAME=2FAuth
      - APP_ENV=local
      - APP_TIMEZONE=Europe/Brussels
      - SITE_OWNER=votre-email@exemple.com
      - APP_KEY=base64:GENEREZ_VOTRE_CLE_ICI_32_CHARS
      - APP_URL=https://votre-url.be
      
      # Utilisation de Redis pour le cache, les sessions et les files d'attente
      - CACHE_DRIVER=redis
      - SESSION_DRIVER=redis
      - QUEUE_DRIVER=redis
      
      # Paramètres de connexion Redis (utilise le nom du service défini plus bas)
      - REDIS_HOST=redis
      - REDIS_PORT=6379
      
      # Configuration Mail
      - MAIL_MAILER=smtp
      - MAIL_HOST=votre.serveur-smtp.com
      - MAIL_PORT=587
      - MAIL_USERNAME=votre-login
      - MAIL_PASSWORD=votre-mot-de-passe
      - MAIL_ENCRYPTION=tls

  # Container Redis ultra-léger basé sur Alpine Linux
  redis:
    image: redis:alpine
    container_name: 2fauth_redis
    restart: always
```
