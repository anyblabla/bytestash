# Mise à jour automatique des serveurs Ubuntu par Cron

Ce guide explique comment automatiser la mise à jour des paquets de votre serveur Ubuntu (APT et Snap) en utilisant un script Bash planifié via une tâche Cron.


• ubuntu
• script
• update
• server
• cron
• crontab

```bash
#!/bin/bash

# Modifications apportées par Blabla Linux : https://link.blablalinux.be
# Disponible sur le wiki : https://wiki.blablalinux.be/fr/update-ubuntu-script-cron

# Fichier journal pour enregistrer le déroulement de la mise à jour
LOGFILE="/var/log/ubuntu_update.log"

# Redirection de toute la sortie (standard et erreur) vers le fichier journal
# La sortie sera ajoutée (>>) à chaque exécution.
exec 1>>$LOGFILE 2>&1

# --- Début du processus ---
echo "======================================================"
echo "Début de la mise à jour Ubuntu Server : $(date)"
echo "======================================================"

# 1. Mise à jour de la liste des paquets APT
echo "--- Étape 1 : apt update (Mise à jour des listes de paquets) ---"
sudo apt update

if [ $? -ne 0 ]; then
    echo "Échec de la mise à jour des listes de paquets. Arrêt du script."
    exit 1
fi

# 2. Mise à niveau des paquets installés (apt upgrade)
echo "--- Étape 2 : apt upgrade (Mise à niveau des paquets) ---"
# Utilisation de -y pour la réponse automatique "oui"
sudo apt upgrade -y

# 3. Suppression des dépendances inutiles (apt autoremove)
echo "--- Étape 3 : apt autoremove (Nettoyage des dépendances et anciens noyaux) ---"
sudo apt autoremove -y

# 4. Mise à jour des paquets Snap (spécifique à Ubuntu)
echo "--- Étape 4 : snap refresh (Mise à jour des packages Snap) ---"
sudo snap refresh

echo "======================================================"
echo "Fin de la mise à jour Ubuntu Server : $(date)"
echo "======================================================"

exit 0
```
