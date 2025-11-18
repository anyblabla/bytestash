# Zipline file upload script

Ce script permet d'envoyer un fichier via un gestionnaire de fichier, directement vers une solution Zipline.

• zipline
• file
• upload
• script
• bash

```bash
#!/bin/bash

# Modifications apportées par Blabla Linux : https://link.blablalinux.be

# Ce script télécharge un fichier (passé en argument $1) vers un service Zipline.
# Assurez-vous de remplacer les valeurs sensibles ci-dessous par les vôtres.

# 1. Vérification de l'argument
if [ -z "$1" ]; then
    echo "Usage: $0 <chemin_vers_fichier>"
    exit 1
fi

# 2. Téléchargement via cURL
# Le jeton d'autorisation et l'URL sont à PERSONNALISER
curl \
  -H "authorization: VOTRE_CLE_D_AUTORISATION_ICI" \
  "https://zipline.mondomaine.com/api/upload" \
  -F "file=@$1;type=$(file --mime-type -b "$1")" \
  -H 'content-type: multipart/form-data' \
  -H "x-zipline-format: random" \
  -H "x-zipline-image-compression-percent: 80" \
  | jq -r .files[0].url \
  | wl-copy

# NOTE: Ce script dépend des outils 'curl', 'jq', 'file', et 'wl-copy' (pour Wayland)
```
