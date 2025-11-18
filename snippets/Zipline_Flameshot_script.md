# Zipline Flameshot script

Script Zipline pour Flameshot. Ce dernier permet d'effectuer une capture d'écran grâce à l'outil Flameshot, et d'envoyer cette dernière directement vers la solution Zipline.

• zipline
• flameshot
• script
• bash
• capture
• screenshot

```bash
#!/bin/bash

# Modifications apportées par Blabla Linux : https://link.blablalinux.be

# 1. Capture d'écran (via Flameshot)
flameshot gui -r > /tmp/screenshot.png

# 2. Téléchargement via cURL (vers Zipline ou service similaire)
# Les informations sensibles suivantes DOIVENT être personnalisées :
# - L'en-tête "authorization" (votre jeton d'API/clé)
# - L'URL du serveur (votre instance Zipline)

curl \
  -H "authorization: VOTRE_CLE_D_AUTORISATION_ICI" \
  "https://zipline.mondomaine.com/api/upload" \
  -F file=@/tmp/screenshot.png \
  -H 'content-type: multipart/form-data' \
  -H x-zipline-image-compression-percent: 80 \
  | jq -r .files[0].url \
  | tr -d '\n' \
  | wl-copy

# NOTE: Ce script dépend des outils 'flameshot', 'curl', 'jq', 'tr', et 'wl-copy' (pour Wayland)
```
