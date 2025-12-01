# Automatiser les update de Proxmox Backup Server - PBS

Cette page de wiki détaille la procédure pour automatiser les mises à jour système de votre hôte Proxmox Backup Server (PBS) à l'aide d'un script Bash et de Cron.


• proxmox
• pbs
• backup
• script
• cron
• gotify
• crontab
• bash

```bash
#!/bin/bash

# Modifications apportées par Blabla Linux : https://link.blablalinux.be
# Disponible sur le wiki : https://wiki.blablalinux.be/fr/update-pbs-script-cron

# Fichier journal pour enregistrer le déroulement de la mise à jour
LOGFILE="/var/log/proxmox_update.log"

# Redirection de toute la sortie (standard et erreur) vers le fichier journal
exec 1>>$LOGFILE 2>&1

# --- Début du processus ---
echo "======================================================"
echo "Début de la mise à jour Proxmox Backup Server (PBS) : $(date)"
echo "======================================================"

# 1. Mise à jour de la liste des paquets (apt-get update)
echo "--- Étape 1 : apt-get update (Mise à jour des listes de paquets) ---"
apt-get update

if [ $? -ne 0 ]; then
    echo "Échec de la mise à jour des listes de paquets. Arrêt du script."
    exit 1
fi

# 2. Mise à niveau des paquets installés (apt-get dist-upgrade)
echo "--- Étape 2 : apt-get dist-upgrade (Mise à niveau des paquets) ---"
apt-get dist-upgrade -y

# 3. Suppression des dépendances inutiles (apt-get autoremove)
echo "--- Étape 3 : apt-get autoremove (Nettoyage des dépendances et anciens noyaux) ---"
apt-get autoremove -y

# 4. Nettoyage du cache APT
echo "--- Étape 4 : apt-get clean (Nettoyage du cache APT) ---"
apt-get clean

echo "======================================================"
echo "Fin de la mise à jour Proxmox Backup Server (PBS) : $(date)"
echo "======================================================"

exit 0
```
