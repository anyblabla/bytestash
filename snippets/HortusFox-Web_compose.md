# HortusFox-Web compose

Fichier "docker-compose.yml" à utiliser pour déployer HortusFox-Web via Docker.

• docker
• compose
• yml
• hortus
• jardin

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
services:
  app:
    image: ghcr.io/danielbrendel/hortusfox-web:latest
    container_name: hortusfox-web
    restart: always
    ports:
      - 8080:80
    volumes:
      - ./app_images:/var/www/html/public/img
      - ./app_logs:/var/www/html/app/logs
      - ./app_backup:/var/www/html/public/backup
      - ./app_themes:/var/www/html/public/themes
      - ./app_migrate:/var/www/html/app/migrations
    environment:
      APP_ADMIN_EMAIL: # votre adresse mail
      APP_ADMIN_PASSWORD: # choisissez un mot de passe
      APP_TIMEZONE: Europe/Brussels # votre timezone
      DB_HOST: db
      DB_PORT: 3306
      DB_DATABASE: hortusfox
      DB_USERNAME: # choisissez un nom utilisateur pour votre base de données (identique que ci-dessous)
      DB_PASSWORD: # choisissez un mot de passe pour votre base de données (identique que ci-dessous)
      DB_CHARSET: utf8mb4
    depends_on:
      - db
  db:
    image: mariadb
    container_name: mariadb
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: # choisissez un mot de passe root pour votre base de données
      MYSQL_DATABASE: hortusfox
      MYSQL_USER: # choisissez un nom utilisateur pour votre base de données (identique que ci-dessus)
      MYSQL_PASSWORD: # choisissez un mot de passe pour votre base de données (identique que ci-dessus)
    ports:
      - 3306:3306
    volumes:
      - ./db_data:/var/lib/mysql
volumes:
  db_data: null
  app_images: null
  app_logs: null
  app_backup: null
  app_themes: null
  app_migrate: null

```
