# Mise à jour automatique des hôtes Proxmox VE

Script Bash pour automatiser la mise à jour des hôtes Proxmox VE.

• script
• bash
• proxmox
• cron

```bash
#!/bin/bash

# Modifications apportées par Blabla Linux : https://link.blablalinux.be
# Disponible sur le wiki : https://wiki.blablalinux.be/fr/update-pve-cron

# Fichier journal pour enregistrer le déroulement de la mise à jour
LOGFILE="/var/log/proxmox_update.log"

# Redirection de toute la sortie (standard et erreur) vers le fichier journal
# La sortie sera ajoutée (>>) à chaque exécution.
exec 1>>$LOGFILE 2>&1

# --- Début du processus ---
echo "======================================================"
echo "Début de la mise à jour Proxmox VE : $(date)"
echo "======================================================"

# 1. Mise à jour de la liste des paquets (apt update)
echo "--- Étape 1 : apt update (Mise à jour des listes de paquets) ---"
apt update

if [ $? -ne 0 ]; then
    echo "Échec de la mise à jour des listes de paquets. Arrêt du script."
    exit 1
fi

# 2. Mise à niveau des paquets installés (apt upgrade)
echo "--- Étape 2 : apt upgrade (Mise à niveau des paquets) ---"
# Utilisation de -y pour la réponse automatique "oui"
apt upgrade -y

# 3. Suppression des dépendances inutiles (apt autoremove)
echo "--- Étape 3 : apt autoremove (Nettoyage des dépendances et anciens noyaux) ---"
# Note : Cela ne supprime pas le noyau nouvellement installé avant le redémarrage.
apt autoremove -y

echo "======================================================"
echo "Fin de la mise à jour Proxmox VE : $(date)"
echo "======================================================"

exit 0
```
