# Gestion de Watchtower dans les conteneurs LXC

Ce script est interactif, et est utilisé pour gérer Watchtower dans des conteneurs LXC fonctionnant sur Proxmox VE. Il permet de vérifier l’état, démarrer, arrêter, redémarrer Watchtower et modifier ses configurations automatiquement.

• watchtower
• docker
• compose
• lxc
• pve
• proxmox

```bash
#!/bin/bash
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
# Guide complet sur le wiki : https://wiki.blablalinux.be/fr/script-gestion-watchtower
# Gestion complète de Watchtower dans LXC

MENU="
===============================================
   Gestion de Watchtower dans les conteneurs LXC
===============================================
 [1] 🔍 Voir l’état actuel de Watchtower
 [2] 🚀 Démarrer Watchtower
 [3] 🛑 Arrêter Watchtower
 [4] 🔁 Redémarrer Watchtower
 [5] 📂 Voir le contenu modifiable du docker-compose.yml de Watchtower
 [6] 🔄 Définir restart policy (always/none)
 [7] ✏️  Modifier WATCHTOWER_NO_STARTUP_MESSAGE (true/false)
 [8] ✏️  Modifier WATCHTOWER_CLEANUP (true/false)
 [9] 📅 Modifier le schedule aléatoire (14h-20h, min multiples de 5)
 [10] 📅 Fixer le même schedule pour tous (6 champs, Spring Cron)
 [11] ✏️  Modifier WATCHTOWER_TIMEOUT
 [Q] ❌ Quitter
"

# Obtenir les LXC en ligne avec Docker
get_running_docker_lxc() {
    pct list | awk 'NR>1 && $2=="running"{print $1}' | while read lxc; do
        if pct exec "$lxc" -- docker ps >/dev/null 2>&1; then
            echo "$lxc"
        fi
    done
}

# Trouver docker-compose.yml de watchtower avec timeout (5s)
find_watchtower_compose() {
    lxc_id=$1
    timeout 5s pct exec "$lxc_id" -- find /root -type f -path "*/watchtower/docker-compose.yml" 2>/dev/null | head -n1
}

# Afficher état Watchtower
status_watchtower() {
    for lxc_id in $(get_running_docker_lxc); do
        compose_file=$(find_watchtower_compose "$lxc_id")
        echo "→ LXC $lxc_id"
        if [ -n "$compose_file" ]; then
            pct exec "$lxc_id" -- docker ps --filter name=watchtower
        else
            echo "Pas de docker-compose.yml trouvé ou recherche expirée."
        fi
    done
    read -rp "Appuyez sur [Entrée] pour revenir au menu..."
}

# Démarrer Watchtower
start_watchtower() {
    for lxc_id in $(get_running_docker_lxc); do
        compose_file=$(find_watchtower_compose "$lxc_id")
        if [ -n "$compose_file" ]; then
            dir=$(dirname "$compose_file")
            pct exec "$lxc_id" -- sh -c "cd $dir && docker compose up -d"
            echo "🚀 Watchtower démarré dans LXC $lxc_id"
        else
            echo "Pas de docker-compose.yml trouvé ou recherche expirée pour LXC $lxc_id."
        fi
    done
    read -rp "Appuyez sur [Entrée] pour revenir au menu..."
}

# Arrêter Watchtower
stop_watchtower() {
    for lxc_id in $(get_running_docker_lxc); do
        compose_file=$(find_watchtower_compose "$lxc_id")
        if [ -n "$compose_file" ]; then
            pct exec "$lxc_id" -- docker stop watchtower >/dev/null 2>&1
            echo "🛑 Watchtower arrêté dans LXC $lxc_id"
        else
            echo "Pas de docker-compose.yml trouvé ou recherche expirée pour LXC $lxc_id."
        fi
    done
    read -rp "Appuyez sur [Entrée] pour revenir au menu..."
}

# Redémarrer Watchtower
restart_watchtower() {
    for lxc_id in $(get_running_docker_lxc); do
        compose_file=$(find_watchtower_compose "$lxc_id")
        if [ -n "$compose_file" ]; then
            dir=$(dirname "$compose_file")
            pct exec "$lxc_id" -- sh -c "cd $dir && docker compose down && docker compose up -d"
            echo "🔁 Watchtower redémarré dans LXC $lxc_id"
        else
            echo "Pas de docker-compose.yml trouvé ou recherche expirée pour LXC $lxc_id."
        fi
    done
    read -rp "Appuyez sur [Entrée] pour revenir au menu..."
}

# Voir le contenu modifiable du docker-compose.yml
view_compose() {
    for lxc_id in $(get_running_docker_lxc); do
        compose_file=$(find_watchtower_compose "$lxc_id")
        echo "→ LXC $lxc_id"
        if [ -n "$compose_file" ]; then
            pct exec "$lxc_id" -- sh -c "grep -E 'restart:|WATCHTOWER_NO_STARTUP_MESSAGE|WATCHTOWER_CLEANUP|WATCHTOWER_SCHEDULE|WATCHTOWER_TIMEOUT' $compose_file"
        else
            echo "Pas de docker-compose.yml trouvé ou recherche expirée pour LXC $lxc_id."
        fi
    done
    read -rp "Appuyez sur [Entrée] pour revenir au menu..."
}

# Modifier une clé dans docker-compose.yml et redémarrer
modify_key_restart() {
    key=$1
    new_value=$2
    for lxc_id in $(get_running_docker_lxc); do
        compose_file=$(find_watchtower_compose "$lxc_id")
        if [ -n "$compose_file" ]; then
            pct exec "$lxc_id" -- sed -i "s|^\s*-\s*$key=.*|      - $key=$new_value|" "$compose_file"
            dir=$(dirname "$compose_file")
            pct exec "$lxc_id" -- sh -c "cd $dir && docker compose down && docker compose up -d"
            echo "✅ $key mis à jour et Watchtower redémarré pour LXC $lxc_id"
        else
            echo "Pas de docker-compose.yml trouvé ou recherche expirée pour LXC $lxc_id."
        fi
    done
    read -rp "Appuyez sur [Entrée] pour revenir au menu..."
}

# Définir restart policy
set_restart_policy() {

    POLICY_MENU="
==============================
   Définir la restart policy
==============================
 [1] always (Redémarrer toujours)
 [2] none (Ne pas redémarrer auto)
 [R] Retour au menu principal
"

    while true; do
        clear
        echo "$POLICY_MENU"
        read -rp "Votre choix : " sub_choice

        case $sub_choice in
            1) new_policy="always" ; break ;;
            2) new_policy="none" ; break ;;
            [Rr]) return ;; # Retourne au menu principal
            *) echo "Option invalide." ; read -rp "Appuyez sur [Entrée] pour continuer..." ;;
        esac
    done

    # Applique la politique choisie à tous les LXC
    for lxc_id in $(get_running_docker_lxc); do
        compose_file=$(find_watchtower_compose "$lxc_id")

        if [ -n "$compose_file" ]; then
            # SED corrigé : utilise 4 ESPACES pour s'assurer de l'indentation correcte sous 'watchtower:' dans le YAML.
            pct exec "$lxc_id" -- sed -i "s/^[[:space:]]*restart: .*/    restart: $new_policy/" "$compose_file"

            dir=$(dirname "$compose_file")
            pct exec "$lxc_id" -- sh -c "cd $dir && docker compose down && docker compose up -d"
            echo "✅ Restart policy définie et Watchtower redémarré dans LXC $lxc_id : $new_policy"
        else
            echo "Pas de docker-compose.yml trouvé ou recherche expirée pour LXC $lxc_id."
        fi
    done

    read -rp "Appuyez sur [Entrée] pour revenir au menu..."
}

# Schedule aléatoire (14h-20h, minutes multiples de 5) pour chaque LXC
random_schedule() {
    for lxc_id in $(get_running_docker_lxc); do
        compose_file=$(find_watchtower_compose "$lxc_id")
        if [ -n "$compose_file" ]; then
            hour=$((RANDOM % 7 + 14))
            minute=$((RANDOM % 12 * 5))
            schedule="0 $minute $hour ? * 5"
            pct exec "$lxc_id" -- sed -i "s|^\s*-\s*WATCHTOWER_SCHEDULE=.*|      - WATCHTOWER_SCHEDULE=$schedule|" "$compose_file"
            dir=$(dirname "$compose_file")
            pct exec "$lxc_id" -- sh -c "cd $dir && docker compose down && docker compose up -d"
            echo "✅ WATCHTOWER_SCHEDULE mis à jour et Watchtower redémarré pour LXC $lxc_id : $schedule"
        else
            echo "Pas de docker-compose.yml trouvé ou recherche expirée pour LXC $lxc_id."
        fi
    done
    read -rp "Appuyez sur [Entrée] pour revenir au menu..."
}

# Schedule fixe pour tous (Spring cron, 6 champs)
fixed_schedule() {
    read -rp "Entrez la valeur du schedule (ex: 0 0 16 ? * 5) : " schedule
    modify_key_restart "WATCHTOWER_SCHEDULE" "$schedule"
}

# Menu principal
while true; do
    clear
    echo "$MENU"
    read -rp "Votre choix : " choice
    case $choice in
        1) status_watchtower ;;
        2) start_watchtower ;;
        3) stop_watchtower ;;
        4) restart_watchtower ;;
        5) view_compose ;;
        6) set_restart_policy ;;
        7) read -rp "Entrez true ou false pour WATCHTOWER_NO_STARTUP_MESSAGE : " val; modify_key_restart "WATCHTOWER_NO_STARTUP_MESSAGE" "$val" ;;
        8) read -rp "Entrez true ou false pour WATCHTOWER_CLEANUP : " val; modify_key_restart "WATCHTOWER_CLEANUP" "$val" ;;
        9) random_schedule ;;
        10) fixed_schedule ;;
        11) read -rp "Entrez la valeur pour WATCHTOWER_TIMEOUT (ex: 30s) : " val; modify_key_restart "WATCHTOWER_TIMEOUT" "$val" ;;
        [Qq]) exit ;;
        *) echo "Option invalide." ; read -rp "Appuyez sur [Entrée] pour continuer..." ;;
    esac
done
```
