# Cinny Web compose

Service : Cinny est un client Matrix web minimaliste et performant.

Installation :
1. Créez un répertoire pour le projet.
2. Déposez le fichier docker-compose.yml dans ce répertoire.
3. Créez un fichier config.json dans le même répertoire avec le contenu fourni.
4. Créez la configuration Nginx dans un bloc pour Nginx Proxy Manager.
5. Lancez le service avec la commande : docker compose up -d.
6. Configurez un reverse proxy (comme Nginx Proxy Manager) pour pointer vers le port 8080 du conteneur. Activez le support des WebSockets dans le proxy.

Fichier config.json :
Le fichier config.json gère la liste des serveurs homeserver disponibles, les espaces et salons mis en avant par défaut, ainsi que le serveur par défaut. Remplacez les valeurs selon vos besoins.

• docker
• compose
• yml
• openwebui
• matrix
• client

```yaml
# =========================================================================
# TRANSMUTE COMPOSE
# Auteur : Amaury aka BlablaLinux (https://blablalinux.be/mes-services-publics/)
# =========================================================================

services:
  cinny:
    image: ghcr.io/cinnyapp/cinny:latest
    container_name: cinny-app
    restart: always
    ports:
      - "8080:80"
    volumes:
      - ./config.json:/usr/share/nginx/html/config.json:ro
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost/"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 10s
```

```json
{
    "defaultHomeserver": 1,
    "homeserverList": [
        "converser.eu",
        "matrix.org",
        "mozilla.org",
        "unredacted.org",
        "xmr.se"
    ],
    "allowCustomHomeservers": true,
    "featuredCommunities": {
        "openAsDefault": false,
        "spaces": [
            "#cinny:matrix.org",
            "#community:matrix.org",
            "#space:unredacted.org",
            "#librewolf-community:matrix.org",
            "#stickers-and-emojis:tastytea.de",
            "#videogames:waywardinn.com",
            "#science-space:matrix.org",
            "#libregaming-games:tchncs.de",
            "#mathematics-on:matrix.org"
        ],
        "rooms": [
            "#tuwunel:grin.hu",
            "#freesoftware:matrix.org",
            "#gentoo:matrix.org"
        ],
        "servers": [
            "matrixrooms.info",
            "matrix.org",
            "mozilla.org",
            "unredacted.org"
        ]
    },
    "hashRouter": {
        "enabled": false,
        "basename": "/"
    }
}
```

```conf
# Règles de réécriture pour Cinny
rewrite ^/config.json$ /config.json break;
rewrite ^/manifest.json$ /manifest.json break;
rewrite ^/sw.js$ /sw.js break;
rewrite ^/pdf.worker.min.js$ /pdf.worker.min.js break;
rewrite ^/public/(.*)$ /public/$1 break;
rewrite ^/assets/(.*)$ /assets/$1 break;
rewrite ^(.+)$ /index.html break;

# Headers de sécurité
proxy_hide_header X-Powered-By;
add_header Referrer-Policy "no-referrer" always;
add_header X-Frame-Options SAMEORIGIN always;
add_header X-Xss-Protection "1; mode=block" always;
```
