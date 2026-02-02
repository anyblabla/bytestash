# KitchenOwl Compose

Solution auto-hébergée de gestion de liste de courses et de recettes. Ce déploiement inclut le frontend, le backend (Python/Flask) et une base de données PostgreSQL. Configuré pour l'envoi d'emails via SMTP et la gestion des politiques de confidentialité personnalisées.

• docker
• compose
• yml
• kitchen
• recipient
• manager

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
# https://docs.kitchenowl.org/latest/self-hosting/advanced/
services:
  kitchenowl-front:
    container_name: kitchenowl_front
    image: tombursch/kitchenowl-web:latest
    restart: always
    ports:
      - "8080:80"
    depends_on:
      - kitchenowl-back
    environment:
      - TZ=Europe/Brussels
      - BACK_URL=kitchenowl-back:5000

  kitchenowl-back:
    container_name: kitchenowl_back
    image: tombursch/kitchenowl-backend:latest
    restart: always
    depends_on:
      - db
    environment:
      - TZ=Europe/Brussels
      - DB_DRIVER=postgresql
      - DB_HOST=db
      - DB_PORT=5432
      - DB_NAME=kitchenowl
      - DB_USER=kitchenowl
      - DB_PASSWORD=${DB_PASSWORD:-votre_mot_de_passe_db}
      - JWT_SECRET_KEY=${JWT_SECRET_KEY:-votre_cle_secrete_jwt}
      - JWT_REFRESH_TOKEN_EXPIRES=30
      - FRONT_URL=https://votre-domaine.tld
      - DISABLE_ONBOARDING=false
      - OPEN_REGISTRATION=true
      - EMAIL_MANDATORY=true
      - PRIVACY_POLICY_URL=https://votre-domaine.tld/privacy
      - TERMS_URL=https://votre-domaine.tld/terms
      - SMTP_HOST=smtp.gmail.com
      - SMTP_PORT=587
      - SMTP_USER=votre-email@gmail.com
      - SMTP_PASS=${SMTP_PASS:-votre_app_password_gmail}
      - SMTP_FROM=votre-email@gmail.com
      - SMTP_REPLY_TO=no-reply@votre-domaine.tld
      - SMTP_TLS=true
    volumes:
      - ./data:/data

  db:
    container_name: kitchenowl_db
    image: postgres:15-alpine
    restart: always
    environment:
      - TZ=Europe/Brussels
      - POSTGRES_DB=kitchenowl
      - POSTGRES_USER=kitchenowl
      - POSTGRES_PASSWORD=${DB_PASSWORD:-votre_mot_de_passe_db}
    volumes:
      - ./db_data:/var/lib/postgresql/data
```
