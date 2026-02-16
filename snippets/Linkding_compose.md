# Linkding compose

Déploiement du gestionnaire de marque-pages Linkding derrière Nginx Proxy Manager (NPM). Cette configuration inclut la gestion des en-têtes de confiance pour éviter les erreurs CSRF 403 et l'initialisation automatique du compte administrateur.

• docker
• compose
• yml
• linkding
• self-hosting
• nginx proxy manager
• admin-system

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
services:
  linkding:
    container_name: linkding
    image: sissbruecker/linkding:latest
    ports:
      - "9090:9090"
    volumes:
      - ./data:/etc/linkding/data
    environment:
      # --- CONFIGURATION REVERSE PROXY ---
      # Remplacez l'URL par votre domaine réel (ex: https://links.domaine.com)
      - LD_CSRF_TRUSTED_ORIGINS=https://votre-domaine.be
      - LD_USE_X_FORWARDED_HOST=true

      # --- INITIALISATION DU COMPTE ---
      # Supprimer ou commenter après la première connexion réussie
      - LD_SUPERUSER_NAME=admin
      - LD_SUPERUSER_PASSWORD=ChangeMePlease$$123

      # Fuseau horaire (ex: Europe/Brussels)
      - TZ=Europe/Brussels
    restart: always
```
