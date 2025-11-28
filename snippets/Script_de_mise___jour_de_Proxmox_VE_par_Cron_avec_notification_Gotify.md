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
PACKAGES_TO_UPGRADE=0 # Nouveau compteur

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
echo "Début du processus Proxmox VE sur $HOSTNAME : $(date)"
echo "======================================================"

# 1. Mise à jour de la liste des paquets (apt-get update)
echo "--- Étape 1 : apt-get update (Mise à jour des listes de paquets) ---"
apt-get update

if [ $? -ne 0 ]; then
    echo "Échec de la mise à jour des listes de paquets. Arrêt du script."
    UPDATE_SUCCESS=1
else
    # 2. Vérification s'il y a des mises à jour disponibles
    echo "--- Étape 2 : apt-get dist-upgrade -s (Simulation de mise à jour) ---"
    # L'option -s simule. On compte le nombre de lignes avec "Inst" (Installé),
    # "Upgr" (Mis à jour) ou "Remv" (Supprimé) pour déterminer si quelque chose se passe.
    # On ajoute --assume-no pour être sûr qu'aucun prompt n'interfère, même si -s devrait suffire.
    # On utilise la sortie standard et on filtre pour compter les actions réelles sur les paquets.
    PACKAGES_TO_UPGRADE=$(apt-get -s --assume-no dist-upgrade 2>/dev/null | grep -E '^(Inst|Upgr|Remv)' | wc -l)
    
    echo "Nombre de paquets à mettre à jour/installer/supprimer : $PACKAGES_TO_UPGRADE"

    if [ "$PACKAGES_TO_UPGRADE" -gt 0 ]; then
        echo "Des mises à jour sont disponibles. Démarrage de l'installation..."
        
        # 3. Mise à niveau des paquets installés (apt-get dist-upgrade)
        echo "--- Étape 3 : apt-get dist-upgrade (Mise à niveau des paquets) ---"
        apt-get dist-upgrade -y

        # 4. Suppression des dépendances inutiles (apt-get autoremove)
        echo "--- Étape 4 : apt-get autoremove (Nettoyage des dépendances et anciens noyaux) ---"
        apt-get autoremove -y

        # 5. Nettoyage du cache APT
        echo "--- Étape 5 : apt-get clean (Nettoyage du cache APT) ---"
        apt-get clean
    else
        echo "Aucune mise à jour de paquet disponible. Les étapes d'installation seront ignorées."
    fi
fi

echo "======================================================"
echo "Fin du processus Proxmox VE : $(date)"
echo "======================================================"

# --- ENVOI DE LA NOTIFICATION FINALE ---

# On envoie la notification UNIQUEMENT si (il y a eu des MAJ) OU (il y a eu un ÉCHEC)
if [ "$PACKAGES_TO_UPGRADE" -gt 0 ] || [ $UPDATE_SUCCESS -ne 0 ]; then
    if [ $UPDATE_SUCCESS -eq 0 ]; then
        # Succès de la mise à jour (et il y avait des MAJ à faire)
        NOTIFICATION_TITLE="✅ PVE Update SUCCÈS sur $HOSTNAME"
        NOTIFICATION_MESSAGE="La mise à jour de $PACKAGES_TO_UPGRADE paquet(s) s'est terminée. Redémarrage nécessaire si nouveau noyau."
        NOTIFICATION_PRIORITY=4 # Priorité moyenne
    else
        # Échec
        NOTIFICATION_TITLE="❌ PVE Update ÉCHEC sur $HOSTNAME"
        NOTIFICATION_MESSAGE="La mise à jour de PVE a ÉCHOUÉ (apt-get update). Consultez $LOGFILE sur l'hôte."
        NOTIFICATION_PRIORITY=8 # Haute priorité
    fi
    
    send_gotify_notification "$NOTIFICATION_TITLE" "$NOTIFICATION_MESSAGE" $NOTIFICATION_PRIORITY
else
    # Aucune mise à jour et pas d'échec
    echo "Aucun paquet mis à jour et aucun échec. Aucune notification Gotify envoyée."
fi

exit $UPDATE_SUCCESS
```
