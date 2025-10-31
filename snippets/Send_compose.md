# Send compose

Fichier "docker-compose.yml" à utiliser pour déployer Send via Docker.

• compose
• docker
• yml
• send
• file
• share

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
services:
  send:
    image: registry.gitlab.com/timvisee/send:latest
    restart: always
    container_name: send
    ports:
      - 1234:1234
    volumes:
      - ./uploads:/uploads
      - ./custom_assets:/app/dist/custom_assets
    environment:
      - VIRTUAL_HOST=send.blablalinux.be
      - VIRTUAL_PORT=1234
      - DHPARAM_GENERATION=false
      - NODE_ENV=production
      - BASE_URL=https://send.blablalinux.be
      - PORT=1234
      - REDIS_HOST=redis
      # For local uploads storage
      - FILE_DIR=/uploads
      # For S3 object storage (disable volume and FILE_DIR variable)
      #- AWS_ACCESS_KEY_ID=********
      #- AWS_SECRET_ACCESS_KEY=********
      #- S3_BUCKET=send
      #- S3_ENDPOINT=s3.us-west-2.amazonaws.com
      #- S3_USE_PATH_STYLE_ENDPOINT=true
      # To customize upload limits
      - EXPIRE_TIMES_SECONDS=3600,86400,259200,432000,604800,2592000,31536000
      - DEFAULT_EXPIRE_SECONDS=86400
      - MAX_EXPIRE_SECONDS=2592000
      - DOWNLOAD_COUNTS=1,2,5,10,15,25,50,100,1000
      - MAX_DOWNLOADS=10
      - DEFAULT_DOWNLOADS=1
      - MAX_FILE_SIZE=1073741824
      - MAX_FILES_PER_ARCHIVE=64
      # To customize web interface
      - UI_COLOR_PRIMARY=#105755
      - UI_COLOR_ACCENT=#f88013
      - UI_CUSTOM_ASSETS_ICON=custom_assets/logo.png
      - UI_CUSTOM_ASSETS_FAVICON_32PX=custom_assets/favicon.png
      - CUSTOM_FOOTER_TEXT=Link Blabla Linux
      - CUSTOM_FOOTER_URL=https://link.blablalinux.be
      - SEND_FOOTER_DMCA_URL=https://blablalinux.be/contact/
  redis:
    image: redis:alpine
    restart: always
    container_name: redis
    volumes:
      - ./send-redis:/data
volumes:
  send-redis: null
```

```plaintext
# Set this up as hourly cronjob to delete old files, that Send forgot to remove
# itself.
#
# Run `crontab -e` and add the line below, and configure the correct path.

# Delete leaked Send upload files older than 7 days (and a bit) every hour
0 * * * * find /var/lib/send/uploads/ -mmin +10130 -exec rm {} \;

# Uploads have their lifetime in days prefixed, so you can be a little bit
# smarter with cleaning up:
#
# 0 * * * * find /var/lib/send/uploads/ -name 7-\* -mmin +10130 -exec rm {} \;
# 0 * * * * find /var/lib/send/uploads/ -name 1-\* -mmin +1500 -exec rm {} \;

# Dans un terminal : crontab -e
# Copier/Coller les lignes suivantes...

0 * * * * find /root/send/uploads/ -name 1-\* -mmin +60 -exec rm {} \;
0 * * * * find /root/send/uploads/ -name 1-\* -mmin +1440 -exec rm {} \;
0 * * * * find /root/send/uploads/ -name 3-\* -mmin +4320 -exec rm {} \;
0 * * * * find /root/send/uploads/ -name 5-\* -mmin +7200 -exec rm {} \;
0 * * * * find /root/send/uploads/ -name 7-\* -mmin +10080 -exec rm {} \;
0 * * * * find /root/send/uploads/ -name \*-\* -mmin +43200 -exec rm {} \;
```
