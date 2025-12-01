# Artalk compose

Fichier "docker-compose.yml" à utiliser pour déployer Artalk via Docker

• comment
• docker
• compose
• yml
• artalk

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
services:
  artalk:
    container_name: artalk
    image: artalk/artalk-go:latest # Pensez à vérifier le tag de l'image (latest recommandé)
    restart: always
    ports:
      - "8080:23366"
    volumes:
      - ./data:/data
      - ./data/artalk-img:/data/artalk-img
    environment:
      - TZ=Europe/Brussels
      - ATK_LOCALE="fr"
      - ATK_SITE_DEFAULT="Mon Wiki"
      - ATK_SITE_URL="https://wiki.mondomaine.com" # À PERSONNALISER : URL de votre site
      - ATK_TIMEZONE="Europe/Brussels"
      - ATK_TRUSTED_DOMAINS="https://wiki.mondomaine.com" # À PERSONNALISER : Domaine(s) de confiance
      - ATK_ADMIN_NOTIFY_EMAIL_MAIL_SUBJECT="[Notification] Nouveau commentaire"
      - ATK_AUTH_EMAIL_ENABLED=true
      - ATK_AUTH_EMAIL_VERIFY_SUBJECT="Votre code de vérification - "
      - ATK_CAPTCHA_ACTION_LIMIT=3
      - ATK_CAPTCHA_ACTION_RESET=60
      - ATK_CAPTCHA_ALWAYS=false
      - ATK_CAPTCHA_CAPTCHA_TYPE="image"
      - ATK_CAPTCHA_ENABLED=true
      - ATK_EMAIL_ALI_DM_ACCOUNT_NAME="email-expediteur@outlook.com" # À PERSONNALISER : Compte d'expédition (si utilisé)
      - ATK_EMAIL_ENABLED=true
      - ATK_EMAIL_MAIL_SUBJECT="[Réponse] Vous avez reçu une réponse"
      - ATK_EMAIL_SEND_ADDR="votre.email.smtp@gmail.com" # À PERSONNALISER : Adresse email d'envoi
      - ATK_EMAIL_SEND_NAME="Artalk Sender"
      - ATK_EMAIL_SEND_TYPE="smtp"
      - ATK_EMAIL_SMTP_HOST=smtp.gmail.com # À PERSONNALISER : Hôte SMTP
      - ATK_EMAIL_SMTP_PASSWORD="MOT DE PASSE SMTP ICI" # À PERSONNALISER : Mot de passe/Clé d'application SMTP
      - ATK_EMAIL_SMTP_PORT=587 # À PERSONNALISER : Port SMTP
      - ATK_EMAIL_SMTP_USERNAME="votre.email.smtp@gmail.com" # À PERSONNALISER : Nom d'utilisateur SMTP
      - ATK_FRONTEND_DARKMODE="auto"
      - ATK_HTTP_BODY_LIMIT=100
      - ATK_HTTP_PROXY_HEADER="X-Forwarded-For $proxy_add_x_forwarded_for"
      - ATK_IMG_UPLOAD_ENABLED=true
      - ATK_IMG_UPLOAD_MAX_SIZE=5
      - ATK_IMG_UPLOAD_PATH="./data/artalk-img/"
      - ATK_LOG_ENABLED=true
      - ATK_LOG_FILENAME="./data/artalk.log"
      - ATK_MODERATOR_AKISMET_KEY="CLE AKISMET ICI" # À PERSONNALISER : Clé Akismet (si utilisée)
      - ATK_MODERATOR_KEYWORDS_ENABLED=false
      - ATK_MODERATOR_KEYWORDS_FILES="[./data/keywords_custom.txt]" # À PERSONNALISER : Chemin vers le fichier de mots-clés (si utilisé)
      - ATK_MODERATOR_KEYWORDS_PENDING=false
      - ATK_MODERATOR_KEYWORDS_REPLACE_TO="x"
      - ATK_MODERATOR_PENDING_DEFAULT=false
```
