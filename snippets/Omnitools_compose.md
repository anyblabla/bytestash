# Omnitools compose

Fichier "docker-compose.yml" à utiliser pour déployer Omnitools via Docker.

• yml
• compose
• docker
• tools

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
services:
  omni-tools:
    image: iib0011/omni-tools:latest
    container_name: omni-tools
    restart: always
    ports:
      - "8080:80"
```
