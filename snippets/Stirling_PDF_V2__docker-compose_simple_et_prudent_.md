# Stirling PDF V2 (docker-compose simple et prudent)

Fichier "docker-compose.yml" à utiliser pour déployer Stirling PDF V2 via Docker.

• compose
• docker
• yml
• stirling
• pdf

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
# Page de wiki disponible à cette adresse : https://wiki.blablalinux.be/fr/stirling-pdf-v2-docker-compose
# Docker compose Stirling PDF V1 : https://bytestash.blablalinux.be/s/f1254114dd45f18e1aba759566f4fc29
services:
  stirling-pdf:
    image: stirlingtools/stirling-pdf:latest-fat
    container_name: stirling-pdf
    restart: always
    ports:
      - 8080:8080
    healthcheck: # Vérification de l'état de l'application
      test:
        - CMD-SHELL
        - curl -f http://localhost:8080/api/v1/info/status | grep -q 'UP' &&
          curl -fL http://localhost:8080/ | grep -qv 'Please sign in'
      interval: 5s
      timeout: 10s
      retries: 16
    volumes:
      - ./tessdata:/usr/share/tessdata
      - ./configs:/configs
      - ./logs:/logs
      - ./pipeline:/pipeline
    environment:
      - DISABLE_ADDITIONAL_FEATURES=false
      - SECURITY_ENABLELOGIN=false # Application ouverte par défaut
      - LANGS=fr_FR
      - SYSTEM_MAXFILESIZE=250
      - SPRING_SERVLET_MULTIPART_MAX_FILE_SIZE=250MB
      - SPRING_SERVLET_MULTIPART_MAX_REQUEST_SIZE=250MB
      #- JAVA_TOOL_OPTIONS="-Xms512m -Xmx6g" # ⚠️ Commenté par défaut (voir section 3.5 de la page de wiki)
      - ALLOW_GOOGLE_VISIBILITY=true
```
