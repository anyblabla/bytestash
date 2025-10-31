# Script de mise à jour de Proxmox VE par Cron avec notification Gotify

Script Cron pour mise à jour Proxmox VE : Automatisez apt-get dist-upgrade et recevez une notification immédiate via Gotify en cas de succès ou d'échec. Inclut les pré-requis curl.


• script
• bash
• proxmox
• cron
• gotify

```bash
#!/bin/bash

# Modifications apportées par Blabla Linux : https://link.blablalinux.be
# Disponible sur le wiki : https://wiki.blablalinux.be/fr/update-pve-script-gotify-cron

# --- PARAMÈTRES DE GOTIFY ---
# IMPORTANT : Remplacez ces valeurs !
GOTIFY_URL="VOTRE_URL_GOTIFY"
GOTIFY_TOKEN="VOTRE_TOKEN_GOTIFY"

# --- PARAMÈTRES DU SCRIPT ---
LOGFILE="/var/log/proxmox_update.log"
HOSTNAME=$(hostname)
UPDATE_SUCCESS=0

# Redirection de toute la sortie vers le fichier journal
exec 1>>$LOGFILE 2>&1

# --- FONCTION DE NOTIFICATION GOTIFY (MÉTHODE FORM-DATA) ---
# Le token est passé dans l'URL et l'option -k est utilisée pour ignorer les erreurs SSL/TLS.
send_gotify_notification() {
    local title="$1"
    local message="$2"
    local priority="$3"

    # Envoi de la notification via curl
    curl -k -s -X POST "$GOTIFY_URL/message?token=$GOTIFY_TOKEN" \
        -F "title=$title" \
        -F "message=$message" \
        -F "priority=$priority" > /dev/null 2>&1
}

# --- DÉBUT DU PROCESSUS DE MISE À JOUR ---
echo "======================================================"
echo "Début de la mise à jour Proxmox VE sur $HOSTNAME : $(date)"
echo "======================================================"

# 1. Mise à jour de la liste des paquets (apt-get update)
echo "--- Étape 1 : apt-get update (Mise à jour des listes de paquets) ---"
apt-get update

if [ $? -ne 0 ]; then
    echo "Échec de la mise à jour des listes de paquets. Arrêt du script."
    UPDATE_SUCCESS=1
else
    # 2. Mise à niveau des paquets installés (apt-get dist-upgrade)
    echo "--- Étape 2 : apt-get dist-upgrade (Mise à niveau des paquets) ---"
    apt-get dist-upgrade -y

    # 3. Suppression des dépendances inutiles (apt-get autoremove)
    echo "--- Étape 3 : apt-get autoremove (Nettoyage des dépendances et anciens noyaux) ---"
    apt-get autoremove -y

    # 4. Nettoyage du cache APT
    echo "--- Étape 4 : apt-get clean (Nettoyage du cache APT) ---"
    apt-get clean
fi

echo "======================================================"
echo "Fin de la mise à jour Proxmox VE : $(date)"
echo "======================================================"

# --- ENVOI DE LA NOTIFICATION FINALE ---

if [ $UPDATE_SUCCESS -eq 0 ]; then
    # Succès
    NOTIFICATION_TITLE="✅ PVE Update SUCCÈS sur $HOSTNAME"
    NOTIFICATION_MESSAGE="La mise à jour PVE s'est terminée. Redémarrage nécessaire si nouveau noyau."
    NOTIFICATION_PRIORITY=4 # Priorité moyenne
else
    # Échec
    NOTIFICATION_TITLE="❌ PVE Update ÉCHEC sur $HOSTNAME"
    NOTIFICATION_MESSAGE="La mise à jour de PVE a ÉCHOUÉ (apt-get update). Consultez $LOGFILE sur l'hôte."
    NOTIFICATION_PRIORITY=8 # Haute priorité
fi

send_gotify_notification "$NOTIFICATION_TITLE" "$NOTIFICATION_MESSAGE" $NOTIFICATION_PRIORITY

exit $UPDATE_SUCCESS
```
