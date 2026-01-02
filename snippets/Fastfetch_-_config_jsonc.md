# Fastfetch - config.jsonc

Fichier de configuration config.jsonc pour Fastfetch, conçu pour offrir un diagnostic système complet en un coup d'œil.

Points forts :

Alignement vertical parfait des flèches pour une lisibilité accrue.
Support hybride : Formatage propre pour les configurations GPU Intel/AMD.
Monitoring : Affichage de la température CPU, de l'IP locale (IPv4) et de l'état de la batterie.
Polyvalent : Identifie précisément l'OS, le noyau, l'environnement de bureau (DE/WM) et les thèmes.
Format JSONC : Permet l'ajout de commentaires pour une personnalisation facile.
Idéal pour les techniciens et les administrateurs système sous environnement Debian/Ubuntu.

Usage : Placer le fichier dans ~/.config/fastfetch/config.jsonc Exécuter la commande : fastfetch

```json
// # Modifications apportées par Blabla Linux : https://link.blablalinux.be
{
    "$schema": "https://github.com/fastfetch-cli/fastfetch/raw/dev/doc/json_schema.json",
    "display": {
        "separator": " ➜  "
    },
    "modules": [
        "title",
        "separator",
        {
            "type": "os",
            "key": "OS          "
        },
        {
            "type": "host",
            "key": "Machine     "
        },
        {
            "type": "kernel",
            "key": "Kernel      "
        },
        {
            "type": "uptime",
            "key": "Uptime      "
        },
        {
            "type": "packages",
            "key": "Packages    "
        },
        {
            "type": "shell",
            "key": "Shell       "
        },
        {
            "type": "display",
            "key": "Display     "
        },
        {
            "type": "de",
            "key": "DE          "
        },
        {
            "type": "wm",
            "key": "WM          "
        },
        {
            "type": "wmtheme",
            "key": "WM Theme    "
        },
        {
            "type": "theme",
            "key": "Theme       "
        },
        {
            "type": "icons",
            "key": "Icons       "
        },
        {
            "type": "font",
            "key": "Font        "
        },
        {
            "type": "cursor",
            "key": "Cursor      "
        },
        {
            "type": "terminal",
            "key": "Terminal    "
        },
        {
            "type": "terminalfont",
            "key": "Term Font   "
        },
        {
            "type": "cpu",
            "key": "CPU         ",
            "temp": true
        },
        {
            "type": "gpu",
            "key": "GPU         ",
            "hideType": "all",
            "format": "{1} {2}"
        },
        {
            "type": "memory",
            "key": "Memory      "
        },
        {
            "type": "swap",
            "key": "Swap        "
        },
        {
            "type": "disk",
            "key": "Disk        "
        },
        {
            "type": "localip",
            "key": "Local IP    ",
            "showIpv6": false
        },
        "break",
        {
            "type": "battery",
            "key": "Battery     "
        },
        {
            "type": "poweradapter",
            "key": "Power       "
        },
        {
            "type": "locale",
            "key": "Locale      "
        },
        "break",
        "colors"
    ]
}
```
