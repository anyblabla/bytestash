# Papra compose

Déploiement complet de Papra (Gestionnaire de documents) via Docker Compose. Inclut le chiffrement AES-256 des documents au repos, l'ingestion automatisée des emails via OwlRelay et un monitoring de santé (healthcheck) natif Node.js.

• docker
• compose
• yml
• papra
• edm
• security
• owlrelay
• sqlite

```yaml
# Source : Amaury aka BlablaLinux (https://link.blablalinux.be)
# Documentation officielle : https://docs.papra.com/
services:
  papra-app:
    image: ghcr.io/papra-hq/papra:latest
    container_name: papra-app
    restart: always
    # L'utilisateur 1000 est recommandé pour la gestion des volumes locaux
    user: "1000:1000"
    ports:
      - "3000:1221"
    environment:
      # Driver SQLite (LibSQL)
      - DATABASE_URL=file:/app/app-data/db/db.sqlite
      
      # Clé secrète pour les sessions (Générer avec : openssl rand -base64 32)
      - AUTH_SECRET=votre_secret_ici
      
      # URL publique de votre instance
      - APP_BASE_URL=https://votre-domaine.com
      - TRUSTED_ORIGINS=https://votre-domaine.com

      # --- CONFIGURATION OCR ---
      - DOCUMENTS_OCR_LANGUAGES=fra,eng
      - DOCUMENT_STORAGE_MAX_UPLOAD_SIZE=52428800 # 50 Mo

      # --- SÉCURITÉ ET LIMITATIONS ---
      # Passer à 'false' impérativement après avoir créé votre compte admin
      - AUTH_FIRST_USER_AS_ADMIN=true
      # 'true' pour une instance publique, 'false' pour restreindre aux invités uniquement
      - AUTH_IS_REGISTRATION_ENABLED=true
      - MAX_ORGANIZATION_COUNT_PER_USER=5
      - MAX_TAGS_PER_ORGANIZATION=100
      # Header pour récupérer l'IP réelle derrière un reverse proxy (NPM, Traefik, etc.)
      - AUTH_IP_ADDRESS_HEADERS=x-forwarded-for

      # --- NETTOYAGE AUTOMATIQUE (CRONS) ---
      # Nombre de jours avant suppression définitive de la corbeille
      - DOCUMENTS_DELETED_DOCUMENTS_RETENTION_DAYS=14
      - ORGANIZATIONS_DELETED_PURGE_DAYS_DELAY=14
      # Exécution des tâches de maintenance (format Cron)
      - DOCUMENTS_HARD_DELETE_EXPIRED_DOCUMENTS_CRON=0 3 * * *
      - ORGANIZATIONS_PURGE_EXPIRED_ORGANIZATIONS_CRON=0 4 * * *

      # --- CONFIGURATION ENVOI EMAILS (SMTP) ---
      - EMAILS_DRIVER=smtp
      - EMAILS_FROM_ADDRESS=Papra <votre-email@gmail.com>
      - SMTP_HOST=smtp.gmail.com
      - SMTP_PORT=587
      - SMTP_USER=votre-email@gmail.com
      - SMTP_PASSWORD=votre_mot_de_passe_application
      - SMTP_SECURE=false

      # --- CHIFFREMENT DES DOCUMENTS ---
      - DOCUMENT_ENCRYPTION_IS_ENABLED=true
      - DOCUMENT_ENCRYPTION_KEY=votre_cle_de_32_caracteres_ici

      # --- CONFIGURATION OWLRELAY (IMPORT MAIL) ---
      - INTAKE_EMAILS_IS_ENABLED=true
      - INTAKE_EMAILS_DRIVER=owlrelay
      - OWLRELAY_API_KEY=votre_cle_api_owlrelay
      - INTAKE_EMAILS_WEBHOOK_SECRET=votre_webhook_secret
      - OWLRELAY_WEBHOOK_URL=https://votre-domaine.com/api/intake-emails/ingest

    volumes:
      - ./app-data:/app/app-data
    healthcheck:
      test: ["CMD-SHELL", "node -e \"const http = require('http'); const req = http.get('http://localhost:1221/', (res) => { if (res.statusCode < 400) process.exit(0); else process.exit(1); }); req.on('error', () => process.exit(1));\""]
      interval: 30s
      timeout: 10s
      retries: 3
```
