# Rackula compose - Sandbox

Version "stateless" (sans persistance serveur) de Rackula. Idéal pour une instance de démonstration publique. Les schémas sont gérés localement dans le navigateur de l'utilisateur. Déploiement ultra-léger et sécurisé (filesystem en lecture seule).

• docker
• compose
• yml
• rackula
• lxc
• sandbox
• demo

```yaml
# =========================================================================
# RACKULA COMPOSE (Version Sandbox / Sans Persistance)
# Auteur : Amaury aka BlablaLinux (https://link.blablalinux.be)
# -------------------------------------------------------------------------
# Note : Cette version ne nécessite pas d'API ni de volume de données.
# Tout le travail est conservé dans le cache local du navigateur.
# =========================================================================

services:
  rackula-demo:
    image: ghcr.io/rackulalives/rackula:latest
    container_name: rackula-demo
    restart: always
    ports:
      - "8080:8080"
    environment:
      - RACKULA_LISTEN_PORT=8080
    networks:
      - rackula-demo-net
    # Sécurité renforcée (Lecture seule)
    security_opt:
      - no-new-privileges:true
    read_only: true
    tmpfs:
      - /var/cache/nginx:size=10M
      - /var/run:size=1M
      - /tmp:size=5M
      - /etc/nginx/conf.d:size=1M,uid=101,gid=101

networks:
  rackula-demo-net:
    driver: bridge
```
