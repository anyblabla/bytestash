# Signature PDF compose

Fichier "docker-compose.yml" à utiliser pour déployer Signature PDF via Docker.



• docker
• compose
• yml
• pdf

```plaintext
git clone https://github.com/24eme/signaturepdf.git
cd signaturepdf
docker build -t signaturepdf .
nano docker-compose.yml
```

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
services:
  signaturepdf:
    image: signaturepdf
    container_name: signaturepdf
    restart: always
    environment:
      - SERVERNAME=signpdf.blablalinux.be
      - UPLOAD_MAX_FILESIZE=48M
      - POST_MAX_SIZE=48M
      - MAX_FILE_UPLOADS=801
      - PDF_STORAGE_PATH=/data
      - DISABLE_ORGANIZATION=false
      - PDF_DEMO_LINK=true
      - DEFAULT_LANGUAGE=fr_FR.UTF-8
      - PDF_STORAGE_ENCRYPTION=false
    ports:
      - '8080:80'
```
