# DocMost compose

Fichier "docker-compose.yml" à utiliser pour déployer DocMost via Docker.

• docker
• yml
• docmost
• compose

```yaml
# Modifications apportées par Blabla Linux : https: //link.blablalinux.be
services:
  docmost:
    image: docmost/docmost:latest
    container_name: docmost
    depends_on:
      - db
      - redis
    environment:
      - APP_URL=http://docmost.blablalinux.be
      - APP_SECRET=6c5f4c9d1f336a3dfc5c630cfc39ff780a856d781597901ebbc73e59ab0e36bb
      - JWT_TOKEN_EXPIRES_IN=30d
      - DATABASE_URL=postgresql://docmost:blablalinux*@db:5432/docmost?schema=public
      - REDIS_URL=redis://redis:6379
      - STORAGE_DRIVER=local
      #- AWS_S3_ACCESS_KEY_ID=
      #- AWS_S3_SECRET_ACCESS_KEY=
      #- AWS_S3_REGION=
      #- AWS_S3_BUCKET=
      #- AWS_S3_ENDPOINT=
      #- AWS_S3_FORCE_PATH_STYLE=
      - MAIL_DRIVER=smtp
      - SMTP_HOST=smtp.gmail.com
      - SMTP_PORT=587
      - SMTP_USERNAME=
      - SMTP_PASSWORD=
      - SMTP_SECURE=false
      - MAIL_FROM_ADDRESS=
      - MAIL_FROM_NAME=Docmost Blabla Linux
      - DRAWIO_URL=https://drawio.blablalinux.be
      - DISABLE_TELEMETRY=false
    ports:
      - 3000:3000
    restart: always
    volumes:
      - ./docmost_storage:/app/data/storage

  db:
    image: postgres:16-alpine
    container_name: postgres
    environment:
      - POSTGRES_DB=docmost
      - POSTGRES_USER=docmost
      - POSTGRES_PASSWORD=blablalinux
    restart: always
    volumes:
      - ./db_data:/var/lib/postgresql/data

  redis:
    image: redis:7.2-alpine
    container_name: redis
    restart: always
    volumes:
      - ./redis_data:/data

volumes:
  docmost_storage:
  db_data:
  redis_data:
```
