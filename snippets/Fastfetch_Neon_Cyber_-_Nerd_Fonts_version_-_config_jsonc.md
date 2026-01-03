# Fastfetch Neon Cyber - Nerd Fonts version - config.jsonc

Variante du style Néon Cyberpunk utilisant des glyphes Nerd Fonts au lieu des émojis pour un rendu haute définition plus sobre.

Une configuration Fastfetch haut de gamme conçue pour les techniciens et administrateurs système. Elle utilise des bannières de couleurs ANSI pour structurer l'information en blocs thématiques instantanément lisibles.

Points forts :
- Design Industriel : Bannières de titres en couleurs inversées (Cyan, Magenta, Vert) pour une structure claire.
- Iconographie Intuitive : Utilisation d'émojis universels pour identifier chaque composant.
- Nettoyage Hardware : Formatage optimisé pour supprimer les mentions constructeurs superflues sur le CPU et le GPU.
- Diagnostic Complet : Monitoring des températures, de l'IPv4 locale et de l'état de la batterie.
- Universel : Le module Host est configuré en mode automatique pour s'adapter à n'importe quelle machine dès l'installation.

Usage : 
- Copier le contenu dans ~/.config/fastfetch/config.jsonc

• fastfetch
• fetch
• jsonc

```json
// # Modifications apportées par Blabla Linux : https://link.blablalinux.be
// # Note : Nécessite l'installation d'une Nerd Font (ex: Hack Nerd Font)
{
    "$schema": "https://github.com/fastfetch-cli/fastfetch/raw/dev/doc/json_schema.json",
    "display": {
        "separator": " ➜ ",
        "color": {
            "keys": "cyan",
            "output": "white"
        }
    },
    "modules": [
        "title",
        {
            "type": "custom",
            "format": " \u001b[46m\u001b[30m ARCHITECTURE SYSTÈME \u001b[0m",
            "key": " "
        },
        {
            "type": "os",
            "key": "   Système  ",
            "format": "{3} {8}"
        },
        {
            "type": "kernel",
            "key": "  󰒋 Noyau    ",
            "format": "{1} {2}"
        },
        {
            "type": "shell",
            "key": "  󱆃 Shell    "
        },
        {
            "type": "packages",
            "key": "  󰏖 Paquets   "
        },
        "break",
        {
            "type": "custom",
            "format": " \u001b[45m\u001b[30m RESSOURCES MATÉRIELLES \u001b[0m",
            "key": " "
        },
        {
            "type": "host",
            "key": "  󰌢 Machine  "
        },
        {
            "type": "cpu",
            "key": "  󰻠 CPU      ",
            "temp": true,
            "format": "{6} @ {7} - {8}"
        },
        {
            "type": "gpu",
            "key": "  󰢮 GPU      ",
            "hideType": "all",
            "format": "{2}"
        },
        {
            "type": "memory",
            "key": "  󰍛 Mémoire  ",
            "format": "{1} / {2} ({3})"
        },
        "break",
        {
            "type": "custom",
            "format": " \u001b[42m\u001b[30m RÉSEAU ET STATUT      \u001b[0m",
            "key": " "
        },
        {
            "type": "localip",
            "key": "  󰩟 IP v4    ",
            "showIpv6": false
        },
        {
            "type": "battery",
            "key": "  󰁹 Énergie  ",
            "format": "{4} ({5})"
        },
        "break",
        "colors"
    ]
}
```
