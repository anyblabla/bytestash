# PwPush compose

Fichier "docker-compose.yml" à utiliser pour déployer PwPush (Password Pusher) via Docker.

• docker
• compose
• yml
• pwpush
• push
• password

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be

services:
  # Base de données PostgreSQL
  db:
    image: postgres:13-alpine
    restart: always
    container_name: postgres
    # Le port 5432 n'est généralement pas nécessaire d'être exposé au public (ports: 5432:5432 a été retiré)
    environment:
      POSTGRES_USER: ${DB_USER:-passwordpusher_user} # Utilisateur par défaut
      POSTGRES_PASSWORD: ${DB_PASSWORD:-please_change_me} # DOIT être changé !
      POSTGRES_DB: ${DB_NAME:-passwordpusher_db}
    volumes:
      - ./postgres:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U ${DB_USER:-passwordpusher_user} -d ${DB_NAME:-passwordpusher_db}"]
      interval: 5s
      timeout: 5s
      retries: 5
      start_period: 10s

  # Application PwPush
  pwpush:
    image: pglombardo/pwpush:stable
    restart: always
    container_name: pwpush
    volumes:
      - ./storage:/opt/PasswordPusher/storage:rw
      - ./logos:/opt/PasswordPusher/public/logos:r
      # L'utilisateur DOIT fournir son propre fichier settings.yml pour la configuration avancée
      - type: bind
        source: ./config/settings.yml
        target: /opt/PasswordPusher/config/settings.yml
    environment:
      PWP__NO_WORKER: true
      # Connexion à la DB utilisant les variables définies ci-dessus
      DATABASE_URL: postgresql://${DB_USER:-passwordpusher_user}:${DB_PASSWORD:-please_change_me}@db:5432/${DB_NAME:-passwordpusher_db}

      # --- PARAMÈTRES GÉNÉRAUX/MARQUE À PERSONNALISER ---
      PWP__THEME: Darkly
      PWP_PRECOMPILE: "true"
      # Les URL des logos et icônes sont remplacées par des exemples génériques ou des variables
      PWP__BRAND__LIGHT_LOGO: ${PWP_LIGHT_LOGO_URL:-https://example.com/logo-light.png}
      PWP__BRAND__DARK_LOGO: ${PWP_DARK_LOGO_URL:-https://example.com/logo-dark.png}
      PWP__BRAND__ICON_57x57: ${PWP_ICON_57:-https://example.com/favicon-57.png}
      PWP__BRAND__ICON_96x96: ${PWP_ICON_96:-https://example.com/favicon-96.png}
      PWP__HOST_DOMAIN: ${PWP_HOST_DOMAIN:-pwpush.example.com}
      PWP__HOST_PROTOCOL: https
      PWP__BRAND__TITLE: Password Pusher
      PWP__BRAND__TAGLINE: Secured by User
      PWP__TIMEZONE: ${TZ:-Europe/Paris} # Changé à Paris/Variable par défaut plus générique
      PWP__DEFAULT_LOCALE: fr
      PWP__SECURE_COOKIES: true
      
      # --- FONCTIONNALITÉS PWP ---
      PWP__DISABLE_SIGNUPS: "false"
      PWP__ENABLE_LOGINS: "true"
      PWP__ALLOW_ANONYMOUS: "true"
      PWP__ENABLE_FILE_PUSHES: "true"
      PWP__FILES__STORAGE: local
      PWP__ENABLE_QR_PUSHES: "true"
      PWP__ENABLE_URL_PUSHES: "true"
      PWP__SHOW_VERSION: "true"
      PWP__SHOW_GDPR_CONSENT_BANNER: "true"
      TURBO_DRIVE_ENABLED: "true"
      
      # --- ANALYTICS (Généralisé) ---
      GA_ENABLE: "false" # Désactivé par défaut (pour la confidentialité)
      GA_ACCOUNT: ${GA_ACCOUNT_ID:-} # À fournir par l'utilisateur s'il active GA_ENABLE
      GA_DOMAIN: ${PWP_HOST_DOMAIN:-pwpush.example.com}
      
      # --- CONFIGURATION EMAIL (À PERSONNALISER) ---
      # Ces lignes doivent ÊTRE remplies par l'utilisateur
      PWP__MAIL__SMTP_ADDRESS: ${SMTP_ADDRESS:-smtp.example.com}
      PWP__MAIL__SMTP_AUTHENTICATION: ${SMTP_AUTH:-login}
      PWP__MAIL__SMTP_USER_NAME: ${SMTP_USER_NAME:-user@example.com} # Votre adresse email a été retirée
      PWP__MAIL__SMTP_PASSWORD: ${SMTP_PASSWORD:-CHANGE_THIS_MAIL_PASSWORD} # Votre mot de passe d'application a été retiré
      PWP__MAIL__SMTP_PORT: ${SMTP_PORT:-587}
      PWP__MAIL__SMTP_STARTTLS: "true"
      PWP__MAIL__SMTP_ENABLE_STARTTLS_AUTO: "true"
      PWP__MAIL__RAISE_DELIVERY_ERRORS: "true"
      PWP__MAIL__OPEN_TIMEOUT: "10"
      PWP__MAIL__READ_TIMEOUT: "10"
      PWP__MAIL__MAILER_SENDER: '"Password Pusher" <${MAILER_SENDER:-notifications@example.com}>'
      
      # --- CLÉ SECRÈTE CRITIQUE ---
      # La clé secrète DOIT être changée. Un générateur peut être utilisé.
      SECRET_KEY_BASE: ${SECRET_KEY_BASE:-GENERATE_A_VERY_LONG_AND_RANDOM_KEY} # Votre clé a été retirée

    ports:
      - 5100:5100
    
    depends_on:
      db:
        condition: service_healthy
        
    healthcheck:
      test:
        - CMD
        - curl
        - -f
        - http://localhost:5100/up
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 40s
```

```plaintext
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
# =======================================================
# FICHIER .env
# Configuration des variables sensibles pour Docker Compose
# Les valeurs entre les guillemets DOIVENT ÊTRE MISES À JOUR
# pour chaque déploiement.
# =======================================================

# --- CLÉS DE BASE DE DONNÉES (POSTGRES) ---
# Ces variables définissent le nom de la base de données, l'utilisateur et le mot de passe.
# Les valeurs DB_USER et DB_NAME sont par défaut, mais le mot de passe DOIT être changé.
DB_USER=passwordpusher_user
DB_PASSWORD="MOT_DE_PASSE_SECURE_POUR_LA_BASE_DE_DONNÉES_A_CHANGER"
DB_NAME=passwordpusher_db

# --- CLÉ SECRÈTE DE L'APPLICATION PWPUSH ---
# C'EST LA CLÉ LA PLUS IMPORTANTE. ELLE EST UTILISÉE POUR CHIFFRER LES SESSIONS ET CRYPTER LES DONNÉES.
# DOIT ÊTRE UNE LONGUE CHAÎNE ALÉATOIRE (64 caractères ou plus) ET UNIQUE.
# Exemple de commande (Linux) : head /dev/urandom | tr -dc A-Za-z0-9 | head -c 128 ; echo ''
SECRET_KEY_BASE="GENERER_UNE_TRES_LONGUE_CLE_SECRETE_ET_LA_METTRE_ICI_OBLIGATOIREMENT"

# --- PARAMÈTRES D'HÉBERGEMENT ET DE MARQUE ---
# Le nom de domaine et le protocole sous lesquels PwPush sera accessible.
PWP_HOST_DOMAIN="pwpush.blablalinux.be"
PWP_HOST_PROTOCOL=https
TZ="Europe/Brussels" # Fuseau horaire (comme vous l'avez spécifié)

# --- LIENS DE LOGO/ICÔNE ---
# Remplacer par les URL de vos logos/icônes.
PWP_LIGHT_LOGO_URL="https://example.com/logo-light.png"
PWP_DARK_LOGO_URL="https://example.com/logo-dark.png"
PWP_ICON_57="https://example.com/favicon-57.png"
PWP_ICON_96="https://example.com/favicon-96.png"

# --- ANALYTICS (Désactivé par défaut dans le compose) ---
# Décommenter et Remplir si GA_ENABLE est mis à "true" dans le docker-compose.yml
# GA_ACCOUNT_ID="G-XXXXXXXXXX"

# --- CONFIGURATION EMAIL SMTP (OBLIGATOIRE POUR LES NOTIFICATIONS) ---
# Remplacer toutes les valeurs ci-dessous par les identifiants de votre serveur SMTP.
SMTP_ADDRESS="smtp.gmail.com"
SMTP_AUTH=login
SMTP_USER_NAME="votre_email@example.com"
SMTP_PASSWORD="MOT_DE_PASSE_D_APPLICATION_SMTP_A_CHANGER"
SMTP_PORT=587
MAILER_SENDER='"Password Pusher" <notifications@example.com>'
```
