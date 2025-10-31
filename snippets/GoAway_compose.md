# GoAway compose

Fichier "docker-compose.yml" à utiliser pour déployer GoAway via Docker.

• docker
• compose
• yml
• dns
• goaway

```yaml
# L'identifiant par défaut est "admin"
# Le mot de passe est généré aléatoirement, et se trouve dans les logs : docker logs goaway

services:
  goaway:
    image: pommee/goaway:0.62.6
    container_name: goaway
    restart: always
    volumes:
      - ./config:/app/config
      - ./data:/app/data
    environment:
      - DNS_PORT=${DNS_PORT:-53}
      - WEBSITE_PORT=${WEBSITE_PORT:-8080}
      #- DOT_PORT=${DOT_PORT:-853}
    ports:
      - ${DNS_PORT:-53}:${DNS_PORT:-53}/udp
      - ${DNS_PORT:-53}:${DNS_PORT:-53}/tcp
      - ${WEBSITE_PORT:-8080}:${WEBSITE_PORT:-8080}/tcp
      #- ${DOT_PORT:-853}:${DOT_PORT:-853}/tcp
    cap_add:
      - NET_BIND_SERVICE
      - NET_RAW
```
