# Rustpad compose

Fichier "docker-compose.yml" à utiliser pour déployer Rustpad via Docker.

• docker
• yml
• compose
• pad
• rustpad

```yaml
# Modifications apportées par Blabla Linux : https: //link.blablalinux.be
version: '3.9'
services:
    rustpad:
        container_name: rustpad
        image: ekzhang/rustpad:latest
        ports:
            - 3030:3030
        environment:
        - EXPIRY_DAYS=7
        - SQLITE_URI=/data/rustpad.db
        volumes:
        - ./data:/data
        restart: always
```
