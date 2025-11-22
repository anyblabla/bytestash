# Script de nettoyage LXC Proxmox avec Gotify

Automatisez le nettoyage de vos LXC (Debian/Ubuntu/Alpine) sur Proxmox. Ce script supprime caches, logs et paquets inutiles, puis envoie un rapport détaillé via Gotify.

• script
• bash
• cleanup
• nettoyage
• lxc
• pve
• proxmox
• gotify

```bash
#!/bin/bash
#
# SCRIPT : clean_lxcs.sh
# OBJECTIF : Nettoyer les caches, logs et paquets inutiles des conteneurs LXC
#            Debian/Ubuntu et Alpine en cours d'exécution.
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
# Guide complet sur le wiki : https://wiki.blablalinux.be/fr/script-nettoyage-lxc-gotify-proxmox
#
# ==============================================================================

# --- PARAMÈTRES DE GOTIFY (À MODIFIER !) ---
GOTIFY_URL="VOTRE_URL_GOTIFY"
GOTIFY_TOKEN="VOTRE_TOKEN_GOTIFY"

# --- PARAMÈTRES DU SCRIPT ---
LOGFILE="/var/log/clean_lxcs_cron.log"
# CTIDs à exclure (séparés par des espaces, ex: "101 105 110")
EXCLUDED_CTIDS=""
SUCCESS_COUNT=0
FAILURE_COUNT=0
CLEANED_CT_COUNT=0
UNSUPPORTED_CT_COUNT=0
HOSTNAME=$(hostname)

exec 1>>$LOGFILE 2>&1

# --- FONCTION DE NOTIFICATION GOTIFY (MÉTHODE FORM-DATA) ---
send_gotify_notification() {
    local title="$1"
    local message="$2"
    local priority="$3"
    curl -k -s -X POST "$GOTIFY_URL/message?token=$GOTIFY_TOKEN" \
        -F "title=$title" \
        -F "message=$message" \
        -F "priority=$priority" > /dev/null 2>&1
}

echo "=================================================="
echo "Démarrage du nettoyage des LXC le $(date)"
echo "=================================================="

# Boucle sur les conteneurs en cours d'exécution
for CTID in $(/usr/sbin/pct list | grep running | awk '{print $1}')
do
    if [[ " ${EXCLUDED_CTIDS} " =~ " ${CTID} " ]]; then
        echo "    [SKIP] Conteneur $CTID exclu manuellement."
        continue
    fi

    echo "--> Traitement du conteneur CTID $CTID..."

    # Déterminer le type d'OS via la configuration Proxmox
    OS_TYPE=$(/usr/sbin/pct config $CTID | awk '/^ostype/ {print $2}')

    # 1. Définition des commandes de nettoyage
    CLEAN_CMD=""

    if [ "$OS_TYPE" == "debian" ] || [ "$OS_TYPE" == "ubuntu" ]; then
        echo "    - Préparation du nettoyage (Debian/Ubuntu)..."
        CLEAN_CMD="
            echo 'Nettoyage des caches (Debian/Ubuntu)...';
            # Nettoyage des caches divers (/var/cache), logs et fichiers temporaires
            find /var/cache -type f -delete 2>/dev/null;
            find /var/log -type f -delete 2>/dev/null;
            find /tmp -mindepth 1 -delete 2>/dev/null;

            # Suppression des paquets inutiles et nettoyage des caches APT
            export DEBIAN_FRONTEND=noninteractive;
            apt-get update -y >/dev/null 2>&1 || true;
            apt-get -y --purge autoremove;
            apt-get -y autoclean;
            apt-get clean;

            # Suppression du cache des listes APT
            rm -rf /var/lib/apt/lists/*;
            echo 'Nettoyage terminé.'
        "

    elif [ "$OS_TYPE" == "alpine" ]; then
        echo "    - Préparation du nettoyage (Alpine)..."
        CLEAN_CMD="
            echo 'Nettoyage des caches (Alpine)...';
            # Nettoyage du cache APK, logs et fichiers temporaires
            apk cache clean;
            find /var/log -type f -delete 2>/dev/null;
            find /tmp -mindepth 1 -delete 2>/dev/null;
            echo 'Nettoyage terminé.'
        "
    else
        echo "    [SKIP] Conteneur $CTID est de type OS $OS_TYPE : non pris en charge."
        UNSUPPORTED_CT_COUNT=$((UNSUPPORTED_CT_COUNT + 1))
        continue
    fi

    # 2. Exécution du nettoyage
    echo "    - Exécution des commandes de nettoyage..."
    /usr/sbin/pct exec $CTID -- bash -c "$CLEAN_CMD"

    if [ $? -ne 0 ]; then
        echo "    [ERREUR CRITIQUE] Le nettoyage du conteneur $CTID a échoué."
        FAILURE_COUNT=$((FAILURE_COUNT + 1))
        continue
    else
        echo "    [OK] Nettoyage du conteneur $CTID réussi."
        SUCCESS_COUNT=$((SUCCESS_COUNT + 1))
        CLEANED_CT_COUNT=$((CLEANED_CT_COUNT + 1))
    fi

done

echo "=================================================="
echo "Fin du nettoyage des LXC le $(date)"
echo "=================================================="

# --- ENVOI DE LA NOTIFICATION FINALE CONDITIONNELLE ---
if [ $CLEANED_CT_COUNT -gt 0 ] || [ $FAILURE_COUNT -gt 0 ] || [ $UNSUPPORTED_CT_COUNT -gt 0 ]; then

    if [ $FAILURE_COUNT -gt 0 ]; then
        TITLE="❌ LXC Nettoyage ÉCHEC(s) sur $HOSTNAME"
        MESSAGE="$FAILURE_COUNT LXC ont rencontré une ERREUR. $CLEANED_CT_COUNT LXC nettoyés. $UNSUPPORTED_CT_COUNT ignorés (OS non supporté)."
        PRIORITY=8
    else
        TITLE="✅ LXC Nettoyage SUCCÈS sur $HOSTNAME"
        MESSAGE="$CLEANED_CT_COUNT LXC nettoyés avec succès. $UNSUPPORTED_CT_COUNT ignorés (OS non supporté)."
        PRIORITY=4
    fi

    send_gotify_notification "$TITLE" "$MESSAGE" $PRIORITY
else
    echo "Aucun conteneur n'a été nettoyé. Aucune notification Gotify envoyée."
fi

exec 1>&- 2>&-
exit 0
```
