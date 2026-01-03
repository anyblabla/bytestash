# Fastfetch Open Dashboard - config.jsonc

Fichier config.jsonc pour Fastfetch offrant un affichage structuré en blocs thématiques (Système, Matériel, Réseau). Ce layout "Open-Ended" a été conçu pour éviter les bugs d'alignement à droite tout en restant parfaitement organisé à gauche.

Points forts :
- Structure visuelle : Sections séparées par des bordures "Box-drawing".
- Nettoyage hardware : Formatage intelligent des noms CPU/GPU (suppression des mentions inutiles).
- Support double GPU : Gestion propre de l'affichage hybride (Intel/AMD).
- Complet : Inclus température CPU, IP locale, batterie et état de l'alimentation.
- Universel : Testé sur Debian 13 (Trixie), idéal pour l'administration et le reconditionnement.

Usage :
- Copier le contenu dans ~/.config/fastfetch/config.jsonc Lancer avec la commande : fastfetch

• fastfetch
• jsonc
• fetch

```json
// # Modifications apportées par Blabla Linux : https://link.blablalinux.be
{
    "$schema": "https://github.com/fastfetch-cli/fastfetch/raw/dev/doc/json_schema.json",
    "display": {
        "separator": " ",
        "color": {
            "keys": "magenta"
        }
    },
    "modules": [
        "title",
        {
            "type": "custom",
            "format": "┌─ System Information ──────────────────────────────────"
        },
        {
            "type": "os",
            "key": "│ 🐧 OS     ",
            "format": "{3} {8}"
        },
        {
            "type": "kernel",
            "key": "│ ⚙️  Kernel ",
            "format": "{1} {2}"
        },
        {
            "type": "uptime",
            "key": "│ ⏱️  Uptime ",
            "format": "{1}{2} {3}{4}"
        },
        {
            "type": "packages",
            "key": "│ 📦 Pkgs   "
        },
        {
            "type": "custom",
            "format": "├─ Hardware & Thermal ──────────────────────────────────"
        },
        {
            "type": "host",
            "key": "│ 💻 Host   "
        }, // Version normale pour le partage
        {
            "type": "cpu",
            "key": "│ 🧠 CPU    ",
            "temp": true,
            "format": "{6} @ {7} - {8}"
        },
        {
            "type": "gpu",
            "key": "│ 🎮 GPU    ",
            "hideType": "all",
            "format": "{2}"
        },
        {
            "type": "memory",
            "key": "│ 💾 RAM    "
        },
        {
            "type": "custom",
            "format": "├─ Network & Storage ───────────────────────────────────"
        },
        {
            "type": "disk",
            "key": "│ 💽 Disk   ",
            "folders": "/"
        },
        {
            "type": "localip",
            "key": "│ 🌐 IPv4   ",
            "showIpv6": false
        },
        {
            "type": "custom",
            "format": "└───────────────────────────────────────────────────────"
        },
        "break",
        "colors"
    ]
}
```
