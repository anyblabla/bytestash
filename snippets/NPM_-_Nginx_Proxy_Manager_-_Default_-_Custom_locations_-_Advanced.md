# NPM - Nginx Proxy Manager - Default - Custom locations - Advanced

Exemple de configuration par défaut pour les onglets "Custom locations" et "Advanced", qui peut convenir pour n'importe quel "Proxy Host" dans NPM (Nginx Proxy Manager).

• npm
• proxy

```plaintext
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
proxy_hide_header X-Powered-By;
add_header Referrer-Policy "no-referrer" always;
add_header X-Frame-Options SAMEORIGIN always;
add_header X-Xss-Protection "1; mode=block" always;
add_header X-Robots-Tag "noindex, noarchive, nofollow" always;
```

```plaintext
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
proxy_set_header Host $host;
proxy_set_header X-Scheme $scheme;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-Host $http_host;
proxy_set_header Connection $http_connection;
proxy_set_header X-Forwarded-Protocol $scheme;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
```
