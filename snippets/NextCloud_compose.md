# NextCloud compose

Déploiement complet de Nextcloud avec une base de données MariaDB. Cette configuration inclut le redémarrage automatique des conteneurs, le stockage des données sur des volumes locaux, ainsi qu'un système de healthcheck pour garantir que l'application ne démarre qu'une fois la base de données opérationnelle.

• docker
• compose
• yml
• nextcloud
• mariadb
• cloud

```yaml
# =========================================================================
# NEXTCLOUD COMPOSE
# Auteur : Amaury aka BlablaLinux (https://link.blablalinux.be/)
# =========================================================================

services:
  db:
    image: mariadb:10.11
    container_name: nextcloud-db
    command: --transaction-isolation=READ-COMMITTED --binlog-format=ROW
    restart: always
    volumes:
      - ./db:/var/lib/mysql
    environment:
      - MYSQL_ROOT_PASSWORD=K9zP#2mX7vR!qL5t
      - MYSQL_PASSWORD=wB4nS@8uY1kE#vH9
      - MYSQL_DATABASE=nextcloud
      - MYSQL_USER=nextcloud
    healthcheck:
      test: ["CMD", "healthcheck.sh", "--connect", "--innodb_initialized"]
      interval: 10s
      timeout: 5s
      retries: 3

  app:
    # NOTE REVERSE PROXY (NPM) :
    # Après installation, éditer ./nextcloud/config/config.php et ajouter :
    # 'overwrite.cli.url' => 'https://votre-domaine.be',
    # 'overwriteprotocol' => 'https',
    # 'trusted_proxies' => ['IP_DE_VOTRE_NPM'],
    image: nextcloud:latest
    container_name: nextcloud-app
    restart: always
    ports:
      - 8080:80
    depends_on:
      db:
        condition: service_healthy
    volumes:
      - ./nextcloud:/var/www/html
    environment:
      - MYSQL_PASSWORD=wB4nS@8uY1kE#vH9
      - MYSQL_DATABASE=nextcloud
      - MYSQL_USER=nextcloud
      - MYSQL_HOST=db
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost/login"]
      interval: 30s
      timeout: 10s
      retries: 5
```
