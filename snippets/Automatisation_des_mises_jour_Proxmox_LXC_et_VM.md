# Automatisation des mises à jour Proxmox (LXC et VM)

Ces scripts Bash permettent d'automatiser la mise à jour des conteneurs LXC et des machines virtuelles (VM) basées sur Debian/Ubuntu sur votre hôte Proxmox VE.

• script
• proxmox
• update
• lxc
• vm

```bash
#!/bin/bash
#
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
# Disponible sur le wiki : https://wiki.blablalinux.be/fr/script-update-lxc-vm-proxmox
#
# SCRIPT : update_vms.sh
# OBJECTIF : Mettre à jour toutes les VMs Debian/Ubuntu en cours d'exécution
#
# ==============================================================================

LOGFILE="/var/log/update_vms_cron.log"
EXCLUDED_VMS="" # IDs de VMs à exclure (séparés par des espaces)

exec 1>>$LOGFILE 2>&1

echo "=================================================="
echo "Démarrage de la mise à jour des VMs le $(date)"
echo "=================================================="

# Commandes internes pour la VM (utilise apt-get pour la stabilité)
UPDATE_COMMAND="export DEBIAN_FRONTEND=noninteractive LC_ALL=C.UTF-8 && \
                apt-get update -y && \
                apt-get full-upgrade -y && \
                apt-get autoremove -y && \
                apt-get clean && \
                snap refresh 2>/dev/null || true"

REBOOT_CHECK_COMMAND="[ -f /var/run/reboot-required ] && echo 'REBOOT_YES' || echo 'REBOOT_NO'"

# Boucle sur les VMs en cours d'exécution
for VMID in $(/usr/sbin/qm list | grep running | awk '{print $1}')
do
    if [[ " $EXCLUDED_VMS " =~ " $VMID " ]]; then
        echo "    [SKIP] VM $VMID exclue."
        continue
    fi

    echo "--> Traitement de la VM VMID $VMID..."

    # 2. Exécution des mises à jour
    echo "    - Exécution des mises à jour..."
    /usr/sbin/qm guest exec $VMID --timeout 300 /bin/bash -- -c "$UPDATE_COMMAND"

    if [ $? -ne 0 ]; then
        echo "    [ERREUR CRITIQUE] La mise à jour de la VM $VMID a échoué. Poursuite vers la prochaine VM."
        continue
    fi

    # 3. Vérification du besoin de redémarrage
    echo "    - Vérification du besoin de redémarrage..."
    REBOOT_CHECK_OUTPUT=$(/usr/sbin/qm guest exec $VMID --timeout 60 /bin/bash -- -c "$REBOOT_CHECK_COMMAND")

    if [[ "$REBOOT_CHECK_OUTPUT" == *"REBOOT_YES"* ]]; then

        echo "    [ALERTE] Redémarrage nécessaire pour la VM $VMID. Redémarrage en cours..."

        # 4. Redémarrage sécurisé
        echo "    - Arrêt de la VM $VMID (shutdown)..."
        /usr/sbin/qm shutdown $VMID --timeout 120

        if /usr/sbin/qm status $VMID | grep -q running; then
            echo "    - Arrêt gracieux échoué. Forçage de l'arrêt (stop)..."
            /usr/sbin/qm stop $VMID
            sleep 5
        fi

        echo "    - Démarrage de la VM $VMID..."
        /usr/sbin/qm start $VMID
        echo "    [OK] Redémarrage de la VM $VMID terminé."

    else
        echo "    [OK] VM $VMID mise à jour avec succès. Aucun redémarrage critique nécessaire."
    fi

done

echo "=================================================="
echo "Fin de la mise à jour des VMs le $(date)"
echo "=================================================="

exec 1>&- 2>&-
```

```bash
#!/bin/bash
#
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
# Disponible sur le wiki : https://wiki.blablalinux.be/fr/script-update-lxc-vm-proxmox
#
# SCRIPT : update_lxcs.sh
# OBJECTIF : Mettre à jour tous les conteneurs LXC en cours d'exécution
#
# ==============================================================================

LOGFILE="/var/log/update_lxcs_cron.log"
EXCLUDED_CTIDS="" # CTIDs à exclure (séparés par des espaces)

exec 1>>$LOGFILE 2>&1

echo "=================================================="
echo "Démarrage de la mise à jour des LXC le $(date)"
echo "=================================================="

# Commandes internes pour le conteneur
UPDATE_COMMAND="export DEBIAN_FRONTEND=noninteractive LC_ALL=C.UTF-8 && \
                apt-get update -y && \
                apt-get full-upgrade -y && \
                apt-get autoremove -y && \
                apt-get clean && \
                snap refresh 2>/dev/null || true"

REBOOT_CHECK_COMMAND="[ -f /var/run/reboot-required ] && echo 'REBOOT_YES' || echo 'REBOOT_NO'"

# Boucle sur les conteneurs en cours d'exécution
for CTID in $(/usr/sbin/pct list | grep running | awk '{print $1}')
do
    if [[ " $EXCLUDED_CTIDS " =~ " $CTID " ]]; then
        echo "    [SKIP] Conteneur $CTID exclu."
        continue
    fi

    echo "--> Traitement du conteneur CTID $CTID..."

    # 2. Exécution des mises à jour
    echo "    - Exécution des mises à jour..."
    /usr/sbin/pct exec $CTID -- bash -c "$UPDATE_COMMAND"

    if [ $? -ne 0 ]; then
        echo "    [ERREUR CRITIQUE] La mise à jour du conteneur $CTID a échoué. Poursuite vers le prochain LXC."
        continue
    fi

    # 3. Vérification du besoin de redémarrage
    echo "    - Vérification du besoin de redémarrage..."
    REBOOT_CHECK_OUTPUT=$(/usr/sbin/pct exec $CTID -- bash -c "$REBOOT_CHECK_COMMAND")

    if [[ "$REBOOT_CHECK_OUTPUT" == *"REBOOT_YES"* ]]; then

        echo "    [ALERTE] Redémarrage nécessaire pour le conteneur $CTID. Redémarrage en cours..."

        # 4. Redémarrage direct (pct reboot)
        /usr/sbin/pct reboot $CTID --timeout 120

        echo "    [OK] Redémarrage du conteneur $CTID terminé."

    else
        echo "    [OK] Conteneur $CTID mis à jour avec succès. Aucun redémarrage critique nécessaire."
    fi

done

echo "=================================================="
echo "Fin de la mise à jour des LXC le $(date)"
echo "=================================================="

exec 1>&- 2>&-
```
