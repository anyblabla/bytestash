# LittleLink-Server compose

Fichier "docker-compose.yml" à utiliser pour déployer LittleLink-Server via Docker.

• compose
• docker
• yml
• link

```yaml
# ------------------------------------------------------------------------------
# Service      : LittleLink Server (Version BlablaLinux)
# Auteur       : Amaury aka BlablaLinux
# Modifications : https://link.blablalinux.be
# Vidéo Démo   : https://peertube.blablalinux.be/w/qcUnPY87GEbuAjiyMxP3s7
# Documentation : https://github.com/techno-tim/littlelink-server/tree/master
# Description   : Configuration optimisée en mode "Full Custom" pour un contrôle 
#                 total de l'ordre, du design et de l'accessibilité.
# ------------------------------------------------------------------------------

services:
  littlelink-server:
    image: ghcr.io/techno-tim/littlelink-server:latest
    container_name: littlelink-server
    
    # --- CONFIGURATION DE L'INTERFACE ---
    environment:
      - META_TITLE=BlablaLinux
      - META_DESCRIPTION=Page de liens BlablaLinux
      - META_AUTHOR=BlablaLinux
      - META_KEYWORDS=Auto-hébergement, Créateur, Réemploi, Proxmox, AdminSys
      - LANG=fr
      - THEME=Dark
      
      # --- AVATAR ET IDENTITÉ ---
      - FAVICON_URL=https://i0.wp.com/blablalinux.be/wp-content/uploads/2025/12/favicon-littlelink-server.jpg
      - AVATAR_URL=https://i0.wp.com/blablalinux.be/wp-content/uploads/2025/12/logo-littlelink-server.jpg
      - AVATAR_2X_URL=https://i0.wp.com/blablalinux.be/wp-content/uploads/2025/12/logo-x2-littlelink-server.jpg
      - AVATAR_ALT=BlablaLinux profils sociaux et services autohébergés
      - NAME=BlablaLinux
      - BIO=🐧 Admin système passionné 🖥️ Spécialiste virtualisation Proxmox & Open Source 🚀
      
      # --- GESTION DES BOUTONS (MODE CUSTOM) ---
      # On force l'ordre uniquement sur "CUSTOM" pour ignorer les boutons par défaut
      - BUTTON_ORDER=CUSTOM
      
      # Liste des noms internes, textes affichés et liens cibles
      - CUSTOM_BUTTON_NAME=Services,Wiki.js,PeerTube,YouTube,WordPress,Cyberlettre,GitHub,Gitea,Matrix,Bluesky,Mastodon,Twitter,Facebook,Tipeee,Emmabuntus,Contactez-moi !
      - CUSTOM_BUTTON_TEXT=Services,Wiki.js,PeerTube,YouTube,WordPress,Cyberlettre,GitHub,Gitea,Matrix,Services,Wiki.js,PeerTube,Emmabuntus,Cyberlettre,Tipeee,Contactez-moi !
      - CUSTOM_BUTTON_URL=https://blablalinux.be/mes-services-publics,https://wiki.blablalinux.be,https://peertube.blablalinux.be/a/blablalinux/videos,https://www.youtube.com/@blablalinux,https://blablalinux.be,https://blablalinux.be/la-cyberlettre,https://github.com/anyblabla,https://gitea.blablalinux.be/blablalinux,https://matrixto.blablalinux.be/#/@blablalinux:blablalinux.be,https://bsky.app/profile/blablalinux.be,https://mastodon.blablalinux.be/@blablalinux,https://x.com/BlablaLinux,https://www.facebook.com/blablalinux,https://fr.tipeee.com/blablalinux/,https://emmabuntus.org,https://blablalinux.be/contact
      
      # --- DESIGN ET ACCESSIBILITÉ ---
      # Couleurs des boutons (Fond et Texte)
      - CUSTOM_BUTTON_COLOR=#1e3a8a,#3b82f6,#f26d15,#FF0000,#21759b,#9333ea,#333333,#609926,#000000,#0085ff,#2b90d9,#1DA1F2,#1877F2,#ed1e3f,#ffcc00,#1a1a1a
      - CUSTOM_BUTTON_TEXT_COLOR=#ffffff,#ffffff,#ffffff,#ffffff,#ffffff,#ffffff,#ffffff,#ffffff,#ffffff,#ffffff,#ffffff,#ffffff,#ffffff,#ffffff,#000000,#ffffff
      
      # Textes alternatifs (SEO & Accessibilité)
      - CUSTOM_BUTTON_ALT_TEXT=Accéder à la liste complète de mes services publics,Consulter le Wiki technique de BlablaLinux,Voir mes vidéos sur mon instance PeerTube,S'abonner à la chaîne YouTube BlablaLinux,Visiter le blog officiel BlablaLinux,S'abonner à la Cyberlettre pour rester informé,Consulter mes dépôts de code sur GitHub,Accéder à mon instance Gitea personnelle,Me rejoindre sur le réseau de communication Matrix,Retrouver BlablaLinux sur Bluesky,Suivre mes actualités sur Mastodon,Suivre BlablaLinux sur X (ex-Twitter),Rejoindre la communauté BlablaLinux sur Facebook,Soutenir financièrement mon travail sur Tipeee,Soutenir le collectif Emmabuntüs,M'envoyer un message ou me contacter
      
      # Icônes FontAwesome
      - CUSTOM_BUTTON_ICON=fa-solid fa-server,fa-solid fa-book-open,fa-solid fa-video,fa-brands fa-youtube,fa-brands fa-wordpress,fa-solid fa-paper-plane,fa-brands fa-github,fa-solid fa-code-branch,fa-solid fa-comments,fa-brands fa-bluesky,fa-brands fa-mastodon,fa-brands fa-x-twitter,fa-brands fa-facebook,fa-solid fa-heart,fa-brands fa-debian,fa-solid fa-envelope
      
      # --- FOOTER ET ANALYTICS ---
      - FOOTER=BlablaLinux © 2025-Présent
      - GA_TRACKING_ID=G-XXXXXXXXXX # <--- Remplacez par votre ID Google Analytics réel
      - SKIP_HEALTH_CHECK_LOGS=true
      
    # --- RÉSEAU ET SÉCURITÉ ---
    ports:
      - 8080:3000 # Accès via http://IP_SERVEUR:8080
    restart: always
    security_opt:
      - no-new-privileges:true
      
    # --- TEST DE SANTÉ ---
    healthcheck:
      test: ["CMD-SHELL", "wget --quiet --tries=1 --spider http://localhost:3000 || exit 1"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 20s
```
