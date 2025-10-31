# GoAccess compose

Fichier "docker-compose.yml" à utiliser pour déployer GoAccess via Docker pour Nginx Proxy Manager via Docker.

• compose
• docker
• yml
• nginx
• npm
• goaccess

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
# Page de wiki disponible à cette adresse : https://wiki.blablalinux.be/fr/docker-compose-goaccess
services:
  goaccess:
    image: 'xavierh/goaccess-for-nginxproxymanager:latest'
    container_name: goaccess
    restart: always
    ports:
      - '7880:7880'
    environment:
      - TZ=Europe/Brussels # optional - à personnaliser
      - LANG=fr_FR.UTF-8 # optional - à personnaliser
      - LANGUAGE=fr_FR.UTF-8 # optional - à personnaliser
      - SKIP_ARCHIVED_LOGS=False # optional
      - DEBUG=False # optional
      - BASIC_AUTH=False # optional
      - BASIC_AUTH_USERNAME=user # optional
      - BASIC_AUTH_PASSWORD=pass # optional   
      - EXCLUDE_IPS=127.0.0.1 # optional - comma delimited 
      - LOG_TYPE=NPM # optional - more information below
      - ENABLE_BROWSERS_LIST=True # optional - more information below
      - CUSTOM_BROWSERS=Kuma:Uptime,TestBrowser:Crawler # optional - comma delimited, more information below
      - HTML_REFRESH=5 # optional - Refresh the HTML report every X seconds. https://goaccess.io/man
      - KEEP_LAST=30 # optional - Keep the last specified number of days in storage. https://goaccess.io/man
      - PROCESSING_THREADS=1 # optional - This parameter sets the number of concurrent processing threads in the program's execution, affecting log data analysis, typically adjusted based on CPU cores. Default is 1. https://goaccess.io/man
    volumes:
      - /data/compose/1/data/logs:/opt/log # à personnaliser - chemin logs nginx proxy manager
      #- /path/to/host/custom:/opt/custom # optional, required if using log_type = CUSTOM

```
