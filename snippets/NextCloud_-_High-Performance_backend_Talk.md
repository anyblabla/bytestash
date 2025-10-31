# NextCloud - High-Performance backend Talk

Fichier "docker-compose.yml" à utiliser pour déployer le backend haute performance Talk pour NextCloud via Docker.

• docker
• compose
• yml
• talk
• nextcloud
• stun
• turn

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
# Ce compose déploit deux serveurs Talk High-Performance backend pour 
# NextCloud. Simplement parce que je dispose de deux serveur NextCloud ! Le 
# serveur NextCloud Blabla Linux et mon serveur NextCloud personnel. Si vous 
# n'avez besoin que d'un serveur Talk High-Performance backend, supprimer 
# toutes les lignes à partir de "nc-talk-ccc:".
services:
  nc-talk-bbl:
    image: ghcr.io/nextcloud-releases/aio-talk:latest
    init: true
    ports:
      - 8181:8081/tcp
      - 3478:3478/tcp
      - 3478:3478/udp
    environment:
      - TZ=Europe/Brussels
      - INTERNAL_SECRET=6caf73de51dfc763bb292ead9e34941255ec904160b01ac741f9f5798d92b399 # openssl rand --hex 32
      - SIGNALING_SECRET=4141fce4d96dd0ee1c4b32cdd126b6091698eb360f12c2eed6fb1d3f67c8bedc # openssl rand --hex 32
      - TURN_SECRET=933bea2cfba8880bd6274513e11302e16790a93da82c52ccc03dd56f884663ac # openssl rand --hex 32
      - TALK_PORT=3478
      - NC_DOMAIN=nextcloud.blablalinux.be # domaine NextCloud
      - TALK_HOST=nextcloud-hpb.blablalinux.be # domaine Talk High-Performance backend
    restart: always
    container_name: nc-talk-bbl
  nc-talk-ccc:
    image: ghcr.io/nextcloud-releases/aio-talk:latest
    init: true
    ports:
      - 8281:8081/tcp
      - 3479:3478/tcp
      - 3479:3478/udp
    environment:
      - TZ=Europe/Brussels
      - INTERNAL_SECRET=7caf73de51dfc764bb292ead9e34941255ec904160b01ac741f9f5798d92b399 # openssl rand --hex 32
      - SIGNALING_SECRET=4241fce4d96dd0ee1c4b33cdd126b6091698eb360f12c2eed6fb1d3f67c8bedc # openssl rand --hex 32
      - TURN_SECRET=933bea2cfba8080bd6274513e01302e16790a93da82c52ccc03dd56f884663ac # openssl rand --hex 32
      - TALK_PORT=3478
      - NC_DOMAIN=ccc.blablalinux.be # domaine NextCloud
      - TALK_HOST=nextcloud-hpb.blablalinux.be # domaine Talk High-Performance backend
    restart: always
    container_name: nc-talk-ccc
```
