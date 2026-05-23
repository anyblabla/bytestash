# PegaProx compose

Ce fichier Docker Compose permet de déployer PegaProx. 

Attention : Avant de lancer le conteneur, vous devez impérativement appliquer les permissions appropriées sur les dossiers de configuration et de logs créés sur votre hôte pour éviter tout problème de droits avec le conteneur.

Exécutez la commande suivante dans le dossier contenant votre fichier docker-compose.yml :
chmod -R 777 ./pegaprox-config ./pegaprox-logs

Pensez également à adapter la variable PEGAPROX_ALLOWED_ORIGINS avec le nom de domaine de votre propre instance.

• docker
• compose
• yml
• dashboard
• proxmox

```yaml
# =========================================================================
# PEGAPROX COMPOSE
# Auteur : Amaury aka BlablaLinux (https://blablalinux.be/mes-services-publics/)
# =========================================================================

services:
  pegaprox:
    image: ghcr.io/pegaprox/pegaprox:latest
    container_name: pegaprox
    healthcheck:
      test: ["CMD", "python3", "-c", "import urllib.request; urllib.request.urlopen('http://localhost:5000/', timeout=5)"]
      interval: 30s
      timeout: 10s
      retries: 3
    ports:
      - "5000:5000"
      - "5001:5001"
      - "5002:5002"
    volumes:
      - ./pegaprox-config:/app/config
      - ./pegaprox-logs:/app/logs
    restart: always
    environment:
      PEGAPROX_PORT: "5000"
      PEGAPROX_HOST: "0.0.0.0"
      PEGAPROX_DEBUG: "false"
      PEGAPROX_API_RATE_LIMIT: "2400"
      PEGAPROX_API_RATE_WINDOW: "60"
      PEGAPROX_SSH_MAX_CONCURRENT: "50"
      PEGAPROX_ALLOWED_ORIGINS: "https://pegaprox.votredomaine.fr"
      PEGAPROX_MAX_REQUEST_SIZE: "10485760"
      PEGAPROX_MAX_UPLOAD_SIZE: "107374182400"
```

```plaintext
Configuration de Nginx Proxy Manager pour PegaProx.

Cette configuration permet de gérer l'hôte proxy principal, les en-têtes de sécurité, le SSL, la compression Gzip, ainsi que les redirections spécifiques pour l'API SSE, les terminaux (Shell) et VNC.

---

1. Onglet "Détails"
- Noms de domaine : pegaprox.votredomaine.fr
- Schéma : http
- Nom d'hôte de redirection / IP : 192.168.x.x
- Port de redirection : 5000
- Liste d'accès : Accessible au public
- Options à cocher :
  [x] Bloquer les exploits courants
  [x] Prise en charge de Websockets

---

2. Onglet "Emplacement personnalisé" (Custom Locations)

Ajouter une première localisation :
- Location : /
- Schéma : http
- Nom d'hôte de redirection / IP : 192.168.x.x
- Port de redirection : 5000
- Configuration avancée (code) :
proxy_hide_header X-Powered-By;
add_header Referrer-Policy "no-referrer" always;
add_header X-Frame-Options SAMEORIGIN always;
add_header X-Xss-Protection "1; mode=block" always;
add_header X-Robots-Tag "noindex, noarchive, nofollow" always;

Ajouter une deuxième localisation :
- Location : /api/sse/
- Schéma : http
- Nom d'hôte de redirection / IP : 192.168.x.x
- Port de redirection : 5000
- Configuration avancée (code) :
proxy_set_header Host $host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
proxy_set_header X-Forwarded-Proto $scheme;
proxy_buffering off;
proxy_cache off;
proxy_read_timeout 86400;

---

3. Onglet "SSL"
- Certificat SSL : Sélectionnez votre certificat (Ex: votredomaine.fr, *.votredomaine.fr)
- Options à cocher :
  [x] Forcer SSL
  [x] HSTS activé
  [x] Prise en charge de HTTP/2
  [x] Sous-domaines HSTS

---

4. Onglet "Avancé" (Configuration Nginx personnalisée)

Copiez-collez le bloc suivant :

# --- Configuration existante (Gzip & Optimisations) ---
gzip on;
gzip_min_length 1000;
gzip_disable "msie6";
gzip_vary on;
gzip_proxied any;
gzip_comp_level 6;
gzip_buffers 16 8k;
gzip_http_version 1.1;
gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;
client_body_buffer_size 512k;
proxy_set_header Host $host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
proxy_set_header X-Forwarded-Proto $scheme;

# --- Ajustements Timeouts et Upload ---
client_max_body_size 100g;
proxy_connect_timeout 10;
proxy_read_timeout 86400s;
proxy_send_timeout 3600;

# --- Configuration Regex pour les WebSockets (en HTTP local) ---
location ~ ^/api/clusters/.*?/vncwebsocket {
    proxy_pass http://192.168.x.x:5001;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_read_timeout 3600;
}

location ~ ^/api/clusters/.*?/shellws {
    proxy_pass http://192.168.x.x:5002;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_read_timeout 3600;
}
```
