# PsiTransfer compose

Fichier "docker-compose.yml" à utiliser pour déployer PsiTransfer via Docker.

• compose
• yml
• docker
• transfer
• file

```yaml
# Modifications apportées par Blabla Linux : https: //link.blablalinux.be
# Page de wiki disponible à cette adresse : https://wiki.blablalinux.be/fr/docker-compose-psitransfer
services:
  psitransfer:
    image: psitrax/psitransfer:latest
    container_name: psitransfer
    restart: always
    ports:
      - 0.0.0.0:3000:3000
    environment:
      - PSITRANSFER_ADMIN_PASS=blablalinux
    volumes:
      - ./data:/data
```
