# Apache Guacamole compose

Fichier "docker-compose.yml" à utiliser pour déployer Apache Guacamole via Docker.

• yml
• compose
• docker
• apache
• guacamole

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
# Page de wiki disponible à cette adresse : https://wiki.blablalinux.be/fr/docker-compose-apache-guacamole
services:
    guacamole_db:
        container_name: guacamole_db
        hostname: guacamole_db
        image: mariadb:10.11
        restart: always
        volumes:
            - ./guacamole_db:/var/lib/mysql
        environment:
            - MYSQL_ROOT_PASSWORD=blablalinux
            - MYSQL_DATABASE=guacamole_db
            - MYSQL_USER=anyblabla
            - MYSQL_PASSWORD=blabla
        expose:
            - 3306

    guacd:
        container_name: guacd
        hostname: guacd
        image: guacamole/guacd:latest
        restart: always
        volumes:
            - ./guacd_drive:/drive:rw
            - ./guacd_record:/record:rw
        expose:
            - 4822

    guacamole:
        container_name: guacamole
        hostname: guacamole
        restart: always
        image: guacamole/guacamole:latest
        depends_on:
            - guacamole_db
            - guacd
        ports:
            - 8080:8080
        links:
            - guacd
        environment:
            - GUACD_HOSTNAME=guacd
            - MYSQL_HOSTNAME=guacamole_db
            - MYSQL_DATABASE=guacamole_db
            - MYSQL_USER=anyblabla
            - MYSQL_PASSWORD=blabla
            - REMOTE_IP_VALVE_ENABLED=true
```
