# Silverbullet compose

Fichier "docker-compose.yml" à utiliser pour déployer Silverbullet via Docker

• docker
• compose
• yml
• wiki
• markdown
• silverbullet

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
services:
  silverbullet:
    image: ghcr.io/silverbulletmd/silverbullet
    restart: always
    environment:
    - SB_USER=admin:admin
    # Pour plus de variables d'environnement : https://silverbullet.md/Install/Configuration
    volumes:
      - ./space:/space
    ports:
      - 3000:3000
  # Pour une mise à jour automatique, installer Watchtower
  #watchtower:
    #image: containrrr/watchtower
    #volumes:
      #- /var/run/docker.sock:/var/run/docker.sock
  # Compose spécial Watchtower : https://bytestash.blablalinux.be/s/aaf7b7d98b36557080aa4870ccf88068

```
