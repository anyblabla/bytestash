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
      # Driver SQLite (LibSQL) - PostgreSQL non supporté actuellement
      - DATABASE_URL=file:/app/app-data/db/db.sqlite
      
      # Clé secrète pour les sessions
      # Générer avec : openssl rand -base64 32
      - AUTH_SECRET=votre_secret_ici
      
      # URL publique de votre instance
      - APP_BASE_URL=https://votre-domaine.com
      - TRUSTED_ORIGINS=https://votre-domaine.com

      # --- CHIFFREMENT DES DOCUMENTS ---
      - DOCUMENT_ENCRYPTION_IS_ENABLED=true
      # Doit contenir exactement 32 caractères
      # Générer avec : openssl rand -hex 16 (pour 32 chars hex) ou une chaîne de 32 caractères
      - DOCUMENT_ENCRYPTION_KEY=votre_cle_de_32_caracteres_ici
      # ---------------------------------

      # --- CONFIGURATION OWLRELAY (IMPORT MAIL) ---
      - INTAKE_EMAILS_IS_ENABLED=true
      - INTAKE_EMAILS_DRIVER=owlrelay
      - OWLRELAY_API_KEY=votre_cle_api_owlrelay
      # Secret pour signer les requêtes entrantes
      # Générer avec : openssl rand -hex 24
      - INTAKE_EMAILS_WEBHOOK_SECRET=votre_webhook_secret
      - OWLRELAY_WEBHOOK_URL=https://votre-domaine.com/api/intake-emails/ingest
      # --------------------------------------------

    volumes:
      - ./app-data:/app/app-data
    healthcheck:
      # Test de santé utilisant Node.js (évite d'installer wget/curl dans l'image)
      test: ["CMD-SHELL", "node -e \"const http = require('http'); const req = http.get('http://localhost:1221/', (res) => { if (res.statusCode < 400) process.exit(0); else process.exit(1); }); req.on('error', () => process.exit(1));\""]
      interval: 30s
      timeout: 10s
      retries: 3
```
