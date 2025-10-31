# Hometube compose

Fichier "docker-compose.yml" à utiliser pour déployer Hometube via Docker.

• compose
• docker
• yml
• tube
• youtube
• hometube
• video

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
services:
  hometube:
    image: ghcr.io/egalitarianmonkey/hometube:latest
    container_name: hometube
    ports:
      - 8501:8501
    environment:
      - PORT=8501
      - TZ=Europe/Brussels
      - UI_LANGUAGE=fr  # en,fr
      - SUBTITLES_CHOICES=fr  # en,fr,es
      - VIDEOS_FOLDER=/data/Videos
      - TMP_DOWNLOAD_FOLDER=/data/tmp
      - YOUTUBE_COOKIES_FILE_PATH=/config/youtube_cookies.txt
      #- DEBUG=false
      #- COOKIES_FROM_BROWSER=vivaldi
      #- BROWSER_SELECT=vivaldi
      #- QUALITY_PROFILE=auto  # auto,mkv_av1_opus,mkv_vp9_opus,mp4_av1_aac,mp4_h264_aac
      #- VIDEO_QUALITY_MAX=max  # max,2160,1440,1080,720,480,360
      #- EMBED_CHAPTERS=true
      #- EMBED_CHAPTERS=true
      #- EMBED_CHAPTERS=keyframes  # keyframes,precise
      #- YTDLP_CUSTOM_ARGS=--max-filesize 5M --write-info-json 
    volumes:
      - ./downloads:/data/Videos
      - ./tmp:/data/tmp
      - ./cookies:/config
    restart: always
```
