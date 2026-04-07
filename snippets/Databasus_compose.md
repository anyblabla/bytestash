# Databasus compose

Configuration Docker Compose pour Databasus, un outil moderne et léger pour automatiser la sauvegarde de vos bases de données (PostgreSQL, MySQL, MariaDB, etc.). Cette version inclut un healthcheck pour surveiller l'état du service et la persistance des données.

• docker
• compose
• yml
• db
• sql
• mariadb
• backup
• mysql
• postgresql
• database

```yaml
# =========================================================================
# DATABASUS COMPOSE
# Auteur : Amaury aka BlablaLinux (https://blablalinux.be/mes-services-publics/)
# =========================================================================

services:
  databasus:
    container_name: databasus
    image: databasus/databasus:latest
    restart: always
    ports:
      - "4005:4005"
    environment:
      - TZ=Europe/Brussels # Fuseau horaire pour la planification des backups
    volumes:
      - ./databasus-data:/databasus-data # Stockage des métadonnées et backups locaux
    healthcheck:
      # Test de l'API interne pour vérifier la disponibilité du service
      test: ["CMD", "curl", "-f", "http://localhost:4005/api/v1/system/health"]
      interval: 30s    
      timeout: 10s     
      retries: 3       
      start_period: 15s
```
