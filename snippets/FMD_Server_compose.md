# FMD Server compose

FMD Server (Find My Device) — 1 LXC Debian dédié, port publié sur l'IP du LXC (<IP_LXC> à adapter). UID container = 1000:1000, chown -R 1000:1000 fmddata/db/ avant le premier démarrage. Config NPM avec client_max_body_size à 15 Mo (limite dure du backend FMD), websockets géré nativement via le toggle NPM.

• docker
• compose
• yml
• nginx
• npm
• fmd
• self-hosted
• lxc

```yaml
# =========================================================================
# FMD SERVER COMPOSE
# Auteur : Amaury aka BlablaLinux (https://blablalinux.be/mes-services-publics/)
# =========================================================================

services:
  fmd:
    image: registry.gitlab.com/fmd-foss/fmd-server:0.16.0
    container_name: fmd
    restart: always

    # NPM est dans un LXC séparé : on publie le port sur l'IP du LXC.
    # Remplacer <IP_LXC> par l'IP du container LXC hébergeant ce service.
    # NPM proxy_pass -> http://<IP_LXC>:8080
    ports:
      - "<IP_LXC>:8080:8080"

    volumes:
      - ./fmddata/db/:/var/lib/fmd-server/db/
      # Décommente si tu veux surcharger la config par défaut :
      # - ./fmddata/config.yml:/etc/fmd-server/config.yml:ro

    # Hardening recommandé par la doc FMD (OWASP)
    read_only: true
    cap_drop: [ALL]
    security_opt: [no-new-privileges]
    tmpfs:
      - /tmp
```

```conf
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
client_max_body_size 15m;
proxy_read_timeout 3600s;

proxy_set_header Host $host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
proxy_set_header X-Forwarded-Proto $scheme;

proxy_hide_header X-Powered-By;
add_header Referrer-Policy "no-referrer" always;
add_header X-Frame-Options SAMEORIGIN always;
add_header X-Xss-Protection "1; mode=block" always;
```
