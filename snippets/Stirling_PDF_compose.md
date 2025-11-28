# Stirling PDF compose

Fichier "docker-compose.yml" à utiliser pour déployer Stirling PDF via Docker.

• compose
• docker
• yml
• stirling
• pdf

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
# Page de wiki disponible à cette adresse : https://wiki.blablalinux.be/fr/docker-compose-stirling-pdf
# Docker compose Stirling PDF V2 : https://bytestash.blablalinux.be/s/00c501cad53b05674c31e329806d987d
services:
  stirlingpdf:
    container_name: stirlingpdf
    image: stirlingtools/stirling-pdf:1.6.0-fat  # Last V1 release!
    restart: always
    healthcheck:
      test:
        - CMD-SHELL
        - curl -f http://localhost:8080/api/v1/info/status | grep -q 'UP' &&
          curl -fL http://localhost:8080/ | grep -qv 'Please sign in'
      interval: 5s
      timeout: 10s
      retries: 16
    ports:
      - 8080:8080
    volumes:
      - ./tessdata:/usr/share/tessdata:rw
      - ./configs:/configs:rw
      - ./customFiles:/customFiles:rw
      - ./logs:/logs:rw
      - ./pipeline:/pipeline:rw
    environment:
      - DOCKER_ENABLE_SECURITY=false
      #- SECURITY_ENABLE_LOGIN=true
      #- SECURITY_INITIALLOGIN_USERNAME="<YOUR_USERNAME>"
      #- SECURITY_INITIALLOGIN_PASSWORD="<YOUR_PASSWORD>"
      #- SECURITY_OAUTH2_ENABLED=true
      #- SECURITY_OAUTH2_AUTOCREATEUSER=false
      #- SECURITY_OAUTH2_ISSUER="<ISSUER_URI>"
      #- SECURITY_OAUTH2_CLIENTID="<CLIENT_ID>"
      #- SECURITY_OAUTH2_CLIENTSECRET="<CLIENT_SECRET>"
      #- SECURITY_OAUTH2_BLOCKREGISTRATION=false
      #- SECURITY_OAUTH2_SCOPES="openid, profile, email"
      #- SECURITY_OAUTH2_USEASUSERNAME=email
      #- SECURITY_OAUTH2_PROVIDER="<PROVIDER>"
      #- SECURITY_SAML2_ENABLED=true
      #- SECURITY_SAML2_AUTOCREATEUSER=false
      #- SECURITY_SAML2_REGISTRATIONID=<REGISTRATION_ID>
      #- SECURITY_SAML2_BLOCKREGISTRATION=false
      #- SECURITY_SAML2_IDPISSUER="<ISSUER_URI>"
      #- SECURITY_SAML2_IDPMETADATAURI=true
      #- SECURITY_SAML2_IDPSINGLELOGINURL="<PROVIDER_SSO_URL>"
      #- SECURITY_SAML2_IDPSINGLELOGOUTURLSECURITY_SAML2_IDPSINGLELOGOUTURL="<PROVIDER_SLO_URL>"
      #- SECURITY_SAML2_IDPCERT="<PROVIDER_SELF-SIGNED_CERTIFICATE>"
      #- SECURITY_SAML2_PRIVATEKEY="<YOUR_PRIVATE_KEY>"
      #- SECURITY_SAML2_SPCERT="<YOUR_CERTIFICATE>"
      - SYSTEM_DEFAULTLOCALE=fr_FR
      - SYSTEM_GOOGLEVISIBILITY=true
      - SYSTEM_ENABLEANALYTICS=true
      - SYSTEM_SHOW_UPDATE=true
      - SYSTEM_SHOW_UPDATE_ONLY_ADMIN=true
      #- SYSTEM_CUSTOM_HTML_FILES=true
      #- UI_APP_NAME=Stirling PDF
      #- UI_HOME_DESCRIPTION=Your locally hosted one-stop-shop for all your PDF needs.
      #- UI_APP_NAVBAR_NAME=Stirling PDF
      - INSTALL_BOOK_AND_ADVANCED_HTML_OPS=false
```
