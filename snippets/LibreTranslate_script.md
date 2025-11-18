# LibreTranslate script

Script Bash pour traduction unifiée (texte ou fichier) avec LibreTranslate. Intégration facile dans Nautilus, Nemo, Thunar, et Dolphin. Affiche les traductions complètes dans une fenêtre Zenity redimensionnable. Installation simple.

• libretranslate
• translate
• script
• nautilus
• nemo
• thunar
• dolphin
• bash

```bash
#!/bin/bash
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
# Page de wiki disponible : https://wiki.blablalinux.be/fr/libretranslate-traduction-unifiee-gestionnaires-fichiers 
# Script unifié : Traduit un fichier (via argument ou variable Nautilus/Nemo) OU le contenu du presse-papiers/sélection (Raccourci).

# --- 1. Configuration de LibreTranslate ---
LIBRETRANSLATE_URL="http://127.0.0.1:5000/translate"
SOURCE_LANG="auto" # Détection automatique
TARGET_LANG="en"   # Langue cible (ajustez si besoin, ex: "fr")
# -------------------------------------

TEXT_TO_TRANSLATE=""
SOURCE_NAME="Texte Sélectionné"

# 2. Détection du contexte

# SCÉNARIO 1 : Lancé depuis un gestionnaire de fichiers comme argument (Thunar, Dolphin)
if [ $# -gt 0 ]; then
    # Prend le premier argument (le chemin du fichier)
    SELECTED_FILE="$1"

    if [ -f "$SELECTED_FILE" ]; then
        TEXT_TO_TRANSLATE=$(cat "$SELECTED_FILE" | tr '\n' ' ')
        SOURCE_NAME="Fichier $(basename "$SELECTED_FILE")"
    fi

# SCÉNARIO 2 : Lancé depuis Nautilus/Nemo (utilise la variable d'environnement)
elif [ -n "$NAUTILUS_SCRIPT_SELECTED_FILE_PATHS" ]; then
    SELECTED_FILE=$(echo "$NAUTILUS_SCRIPT_SELECTED_FILE_PATHS" | head -n 1)

    if [ -f "$SELECTED_FILE" ]; then
        # Lit le contenu du fichier et le met sur une seule ligne
        TEXT_TO_TRANSLATE=$(cat "$SELECTED_FILE" | tr '\n' ' ')
        SOURCE_NAME="Fichier $(basename "$SELECTED_FILE")"
    fi
else
    # SCÉNARIO 3 : Lancé par un raccourci personnalisé (Sélection/Presse-papiers)

    # 2.1. Essayer Wayland (wl-clipboard)
    if command -v wl-paste &> /dev/null; then
        # Tenter la SÉLECTION SIMPLE (texte en surbrillance)
        TEXT_TO_TRANSLATE=$(wl-paste --primary)

        # Si vide, se rabattre sur le PRESSE-PAPIERS (Ctrl+C)
        if [ -z "$TEXT_TO_TRANSLATE" ]; then
             TEXT_TO_TRANSLATE=$(wl-paste)
        fi

    # 2.2. Essayer X11 (xclip)
    elif command -v xclip &> /dev/null; then
        # Tenter la SÉLECTION SIMPLE
        TEXT_TO_TRANSLATE=$(xclip -selection primary -o)

        # Si vide, se rabattre sur le PRESSE-PAPIERS
        if [ -z "$TEXT_TO_TRANSLATE" ]; then
             TEXT_TO_TRANSLATE=$(xclip -selection clipboard -o)
        fi
    else
        notify-send "Traduction LibreTranslate" "Erreur : Dépendance 'wl-clipboard' ou 'xclip' manquante."
        exit 1
    fi
fi

# 3. Vérification du contenu à traduire
if [ -z "$TEXT_TO_TRANSLATE" ]; then
    MESSAGE="Le contenu est vide. Sélectionnez un fichier, ou copiez/sélectionnez du texte."
    notify-send "Traduction LibreTranslate" "$MESSAGE"
    exit 1
fi

# 4. Exécution de la traduction via l'API (sans clé API)
TRANSLATION_JSON=$(curl -s -X POST "$LIBRETRANSLATE_URL" \
     -H 'Content-Type: application/json' \
     -d "{
      \"q\": \"$TEXT_TO_TRANSLATE\",
      \"source\": \"$SOURCE_LANG\",
      \"target\": \"$TARGET_LANG\"
     }")

# 5. Extraction du texte traduit
TRANSLATED_TEXT=$(echo "$TRANSLATION_JSON" | jq -r '.translatedText')

# 6. Affichage du résultat via Zenity (texte complet)
if [ "$TRANSLATED_TEXT" != "null" ] && [ -n "$TRANSLATED_TEXT" ]; then

    # Utilise Zenity pour afficher le texte intégral dans une fenêtre redimensionnable
    zenity --text-info \
        --title="Traduction ($SOURCE_NAME)" \
        --width=600 \
        --height=400 \
        --filename=<(echo -e "--- Traduction complète ---\n\n$TRANSLATED_TEXT")

else
    notify-send "Erreur de Traduction" "Impossible de traduire. Vérifiez votre service LibreTranslate ou la connexion."
fi
```
