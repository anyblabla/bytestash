# Cronmaster compose

Gestionnaire d'interface pour tâches Cron

CronMaster est une interface web puissante et intuitive pour gérer vos tâches planifiées sur Linux. Grâce à lui, visualisez vos crontabs, surveillez l'exécution de vos scripts en temps réel et consultez vos logs sans avoir à utiliser la ligne de commande.

Points forts :
Lisibilité : Syntaxe cron traduite en langage humain.
Logs centralisés : Suivi complet des sorties (stdout/stderr) et codes de sortie.
Simplicité : Gestion via une interface web propre et légère.
Flexibilité : Supporte l'exécution manuelle, le clonage et la sauvegarde de tâches.

Idéal pour les administrateurs système et les passionnés de serveurs sous Proxmox ou Debian qui souhaitent garder un œil sur leurs automatisations.

• docker
• compose
• yml
• cron
• ui
• webui

```yaml
# Référence des variables d'environnement : https://github.com/fccview/cronmaster/blob/main/howto/ENV_VARIABLES.md
# =========================================================================
# CRONMASTER COMPOSE
# Auteur : Amaury aka BlablaLinux (https://blablalinux.be/mes-services-publics/)
# =========================================================================
services:
  cronmaster:
    image: ghcr.io/fccview/cronmaster:latest
    container_name: cronmaster
    user: "root" # Requis pour accéder et modifier les crontabs système
    ports:
      - "40123:3000"
    environment:
      - NODE_ENV=production
      #- APP_URL=https://cronmaster.blablalinux.be # URL de base pour la redirection OIDC
      - LOCALE=fr # Langue de l'interface (supporte fr.json)
      - HOME=/home
      - AUTH_PASSWORD=CHANGER_VOTRE_MOT_DE_PASSE_ICI # Mot de passe d'accès à l'interface
      - HOST_CRONTAB_USER=root,any # Définit les utilisateurs autorisés à gérer les crontabs
      - NEXT_PUBLIC_CLOCK_UPDATE_INTERVAL=30000
      - LIVE_UPDATES=true
      - DISABLE_SYSTEM_STATS=false
      - MAX_LOG_AGE_DAYS=30 # Rétention des logs en jours
      - NEXT_PUBLIC_MAX_LOG_AGE_DAYS=30
      - MAX_LOGS_PER_JOB=30 # Nombre maximum de fichiers de log conservés par tâche
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock # Indispensable pour la communication avec le démon Docker
      - ./scripts:/app/scripts
      - ./data:/app/data
      - ./snippets:/app/snippets
    pid: "host" # Nécessaire pour surveiller les processus hôtes
    privileged: true # Nécessaire pour les permissions de lecture/écriture des crontabs
    restart: always # My restart will always be.
    init: true
```
