# Fastfetch Proxmox - config.jsonc

Une configuration Fastfetch exclusive, optimisée chirurgicalement pour les hyperviseurs Proxmox VE. Elle transforme l'affichage standard en un véritable tableau de bord d'administration, idéal pour un accueil visuel lors de vos connexions SSH.

Points forts :
- Identité Visuelle : Intégration du logo ASCII officiel Proxmox avec une palette de couleurs orange/ambre (38;5;208) pour une immersion totale.
- Métriques Spécifiques PVE : Extraction dynamique de la version précise de Proxmox via une commande pveversion intégrée.
- Segmentation Logique : Organisation des données en trois blocs thématiques (Hyperviseur, Ressources Physiques, Réseau) via des bannières ANSI contrastées.
- Monitoring Critique : Affichage en temps réel de la charge système (loadavg), du nombre de processus et de l'occupation précise des disques.
- Alignement "Pixel-Perfect" : Gestion rigoureuse des espaces pour assurer un alignement parfait des séparateurs ➜, même avec des polices à chasse variable.

Usage :
- Copier le contenu dans ~/.config/fastfetch/proxmox.jsonc
- Lancer avec la commande : fastfetch -c ~/.config/fastfetch/proxmox.jsonc

• fastfetch
• fetch
• jsonc
• proxmox
• pve

```json
// # Modifications apportées par Blabla Linux : https://link.blablalinux.be
{
    "$schema": "https://github.com/fastfetch-cli/fastfetch/raw/dev/doc/json_schema.json",
    "logo": {
        "source": "proxmox",
        "color": {
            "1": "38;5;208",
            "2": "38;5;214"
        },
        "padding": {
            "top": 2,
            "left": 2
        }
    },
    "display": {
        "separator": " ➜  ",
        "color": {
            "keys": "38;5;208",
            "output": "white"
        }
    },
    "modules": [
        "title",
        {
            "type": "custom",
            "format": "\u001b[48;5;208m\u001b[30m INFOS HYPERVISEUR     \u001b[0m"
        },
        {
            "type": "os",
            "key": "  🐧 Système      "
        },
        {
            "type": "command",
            "key": "  💎 PVE Ver      ",
            "shell": "bash",
            "text": "pveversion | cut -d'/' -f2"
        },
        {
            "type": "host",
            "key": "  💻 Machine      "
        },
        {
            "type": "kernel",
            "key": "  ⚙️  Noyau        "
        },
        {
            "type": "uptime",
            "key": "  ⏱️  Activité     "
        },
        {
            "type": "packages",
            "key": "  📦 Paquets      "
        },
        {
            "type": "shell",
            "key": "  🐚 Shell        "
        },
        "break",
        {
            "type": "custom",
            "format": "\u001b[48;5;208m\u001b[30m RESSOURCES PHYSIQUES  \u001b[0m"
        },
        {
            "type": "cpu",
            "key": "  🧠 CPU          ",
            "temp": true
        },
        {
            "type": "gpu",
            "key": "  🎮 GPU          "
        },
        {
            "type": "memory",
            "key": "  💾 RAM          "
        },
        {
            "type": "swap",
            "key": "  🔄 Swap         "
        },
        {
            "type": "disk",
            "key": "  💽 Stockage     ",
            "folders": "/"
        },
        {
            "type": "loadavg",
            "key": "  📈 Charge       "
        },
        {
            "type": "processes",
            "key": "  🔢 Processus    "
        },
        "break",
        {
            "type": "custom",
            "format": "\u001b[48;5;208m\u001b[30m RÉSEAU ET ACCÈS       \u001b[0m"
        },
        {
            "type": "localip",
            "key": "  🌐 IP Admin     ",
            "showIpv6": false
        },
        {
            "type": "dns",
            "key": "  🔍 DNS          "
        },
        {
            "type": "publicip",
            "key": "  🌍 IP Publique  "
        }
    ]
}
```
