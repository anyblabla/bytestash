# Privatebin compose

Fichier "docker-compose.yml" à utiliser pour déployer Privatebin via Docker.

• docker
• yml
• compose
• privatebin
• text

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
services:
  privatebin:
    image: privatebin/nginx-fpm-alpine
    container_name: privatebin
    restart: always
    read_only: true
    #user: "1000:1000"  # Run the container with the UID:GID of your Docker user
    environment:
      - TZ=Europe/Brussels
      - PHP_TZ=Europe/Brussels
    ports:
      - "8082:8080"
    volumes:
      - ./privatebin-data:/srv/data
      - ./conf.php:/srv/cfg/conf.php:ro # second volume for custom configuration file

```
