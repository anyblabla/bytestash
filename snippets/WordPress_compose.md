# WordPress compose

Déploiement complet de WordPress avec MariaDB. Inclut des tests de santé (healthchecks), une politique de redémarrage automatique et l'utilisation de volumes locaux (bind mounts). Configuré pour être exposé derrière un reverse proxy comme Nginx Proxy Manager.

• docker
• compose
• yml
• wordpress
• mariadb
• blog
• cms

```yaml
# =========================================================================
# WORDPRESS COMPOSE
# Auteur : Amaury aka BlablaLinux (https://link.blablalinux.be)
# =========================================================================

# 1. GESTION DES DROITS (Avant de lancer le docker compose) :
# mkdir wp_data db_data
# sudo chown -R 33:33 wp_data
# =========================================================================

# 2. NOTE POUR LE SSL (Nginx Proxy Manager) :
# Si le CSS ne charge pas ou pour forcer le HTTPS, modifiez wp-config.php :
#
# define('FORCE_SSL_ADMIN', true);
# if (isset($_SERVER['HTTP_X_FORWARDED_PROTO']) && $_SERVER['HTTP_X_FORWARDED_PROTO'] === 'https') {
#     $_SERVER['HTTPS'] = 'on';
# }
# =========================================================================

services:
  db:
    image: mariadb:10.11
    container_name: wordpress_db
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: P4ss_W0rd_LinuX_2026!
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wpuser
      MYSQL_PASSWORD: P4ss_W0rd_LinuX_2026!
    healthcheck:
      test: ["CMD", "healthcheck.sh", "--connect", "--innodb_initialized"]
      interval: 10s
      timeout: 5s
      retries: 5
    volumes:
      - ./db_data:/var/lib/mysql

  wordpress:
    depends_on:
      db:
        condition: service_healthy
    image: wordpress:latest
    container_name: wordpress_app
    restart: always
    ports:
      - "8080:80"
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: wpuser
      WORDPRESS_DB_PASSWORD: P4ss_W0rd_LinuX_2026!
      WORDPRESS_DB_NAME: wordpress
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost/"]
      interval: 30s
      timeout: 10s
      retries: 3
    volumes:
      - ./wp_data:/var/www/html
```
