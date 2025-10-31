# Watchtower compose

Fichier "docker-compose.yml" à utiliser pour déployer Watchtower via Docker.

• docker
• compose
• yml
• watch
• boot

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
services:
  watchtower:
    image: containrrr/watchtower:latest
    container_name: watchtower
    restart: always
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
    environment:
      - WATCHTOWER_NO_STARTUP_MESSAGE=false  # N'envoyez pas de message après le démarrage de Watchtower. Sinon, une notification d'information s'affichera.
      - TZ=Europe/Brussels
      - WATCHTOWER_CLEANUP=true  # Supprime les anciennes images après la mise à jour
      #- WATCHTOWER_POLL_INTERVAL=10800  # Intervalle d'interrogation (en secondes). Cette valeur contrôle la fréquence à laquelle Watchtower interroge les nouvelles images
      - WATCHTOWER_SCHEDULE=0 0 10 ? * WED  # Intervalle d'interrogation via cron (syntax spring - ici chaque mercredi à 10 h). Cette valeur contrôle la fréquence à laquelle Watchtower interroge les nouvelles images
      - WATCHTOWER_DISABLE_CONTAINERS=  # Surveiller et mettre à jour les conteneurs dont les noms ne figurent pas dans un ensemble de noms donné (liste séparée par des virgules ou des espaces)
      - WATCHTOWER_TIMEOUT=30s  # Délai d'attente avant l'arrêt forcé du conteneur
      #- WATCHTOWER_NOTIFICATIONS=gotify
      #- WATCHTOWER_NOTIFICATION_GOTIFY_URL=
      #- WATCHTOWER_NOTIFICATION_GOTIFY_TOKEN=
      #- WATCHTOWER_NOTIFICATIONS_HOSTNAME=

```
