# Palmr compose 

Fichier "docker-compose.yml" à utiliser pour déployer Palmr via Docker.

• docker
• compose
• yml
• share
• file
• cloud

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
services:
  palmr:
    image: kyantech/palmr:latest
    container_name: palmr
    restart: always
    ports:
      - 5487:5487 # Web interface
      #- 3333:3333 # API (optional)
    environment:
      # Optional: Uncomment and configure as needed (if you don`t use, you can remove)
      #- ENABLE_S3=true # Set to true to enable S3-compatible storage
      #- DISABLE_FILESYSTEM_ENCRYPTION=true # Set to false to enable file encryption
      #- ENCRYPTION_KEY=your-secure-key-min-32-chars # Required only if encryption is enabled
      #- PALMR_UID=1000 # UID for the container processes (default is 1000)
      #- PALMR_GID=1000 # GID for the container processes (default is 1000)
      - SECURE_SITE=true # Set to true if you are using a reverse proxy
      - DEFAULT_LANGUAGE=fr-FR # Default language for the application (optional, defaults to en-US)
    volumes:
      - ./palmr_data:/app/server
    healthcheck:
      test:
        - CMD
        - curl
        - -f
        - http://localhost:5487
      interval: 30s
      timeout: 10s
      retries: 3
volumes:
  palmr_data: null
```
