# Batterie faible

Script à exécuter sur laptop au démarrage, afin d'obtenir une notification lorsque la batterie atteint 20 % d'autonomie.

• batterie
• sh
• bash
• script

```bash
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
#!/bin/bash

while true
do
    battery_level=$(cat /sys/class/power_supply/BAT0/capacity)

    if [ $battery_level -lt 15 ]
    then
        notify-send "Batterie Faible" "Niveau de batterie actuel : $battery_level%"
    fi

    sleep 60  # Vérifie toutes les minutes
done

```
