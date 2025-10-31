# MinIO S3 Object Storage compose

Fichier "docker-compose.yml" à utiliser pour déployer MinIO S3 Object Storage via Docker.

• yml
• docker
• compose
• minio

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
services:
  minio:
    container_name: minio
    image: 'minio/minio:latest'
    restart: always
    ports:
      - "9000:9000"
      - "9001:9001"
    environment:
      MINIO_ROOT_USER: nom-utilisateur # nom utilisateur personnel
      MINIO_ROOT_PASSWORD: mot-de-passe # mot de passe personnel
      MINIO_BROWSER_REDIRECT_URL: https://minio-ui.exemple.com # domaine de l'instance web minio
      MINIO_SERVER_URL: https://minio.exemple.com # domaine de l'instance api minio
    volumes:
      - ./data:/data
    command: server /data --console-address ":9001"
    healthcheck:
      test:
        - CMD
        - curl
        - '-f'
        - 'http://localhost:9000/minio/health/live'
      retries: 3
      timeout: 5s

```
