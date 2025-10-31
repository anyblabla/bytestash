# OnlyOffice Docs Community compose

Fichier "docker-compose.yml" à utiliser pour déployer OnlyOffice Docs Community via Docker.

• yml
• docker
• onlyoffice
• compose

```yaml
# Modifications apportées par Blabla Linux : https: //link.blablalinux.be
services:
  documentserver:
    image: onlyoffice/documentserver:latest
    environment:
      - JWT_SECRET=60e52cee9b1fc64bd9bdff77a958ee0445b01d84c3ff030986885eea2df91e61
    volumes:
      - ./db:/var/lib/postgresql
      - ./lib:/var/lib/onlyoffice
      - ./data:/var/www/onlyoffice/Data
      - ./logs:/var/log/onlyoffice
    container_name: onlyoffice
    restart: always
    ports:
      - 80:80
    tty: true
    stdin_open: true
```
