# Fastfetch Neon Cyber - config.jsonc

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
            "format": " \u001b[46m\u001b[30m SYSTEM ARCHITECTURE \u001b[0m",
            "key": " "
        },
        {
            "type": "os",
            "key": "  🐧 OS     ",
            "format": "{3} {8}"
        },
        {
            "type": "kernel",
            "key": "  ⚙️  Kernel ",
            "format": "{1} {2}"
        },
        {
            "type": "shell",
            "key": "  🐚 Shell  "
        },
        {
            "type": "packages",
            "key": "  📦 Pkgs   "
        },
        "break",
        {
            "type": "custom",
            "format": " \u001b[45m\u001b[30m HARDWARE RESOURCES  \u001b[0m",
            "key": " "
        },
        {
            "type": "host",
            "key": "  💻 Host   "
        },
        {
            "type": "cpu",
            "key": "  🧠 CPU    ",
            "temp": true,
            "format": "{6} @ {7} - {8}"
        },
        {
            "type": "gpu",
            "key": "  🎮 GPU    ",
            "hideType": "all",
            "format": "{2}"
        },
        {
            "type": "memory",
            "key": "  💾 RAM    ",
            "format": "{1} / {2} ({3})"
        },
        "break",
        {
            "type": "custom",
            "format": " \u001b[42m\u001b[30m NETWORK & STATUS    \u001b[0m",
            "key": " "
        },
        {
            "type": "localip",
            "key": "  🌐 IPv4   ",
            "showIpv6": false
        },
        {
            "type": "battery",
            "key": "  🔋 Power  ",
            "format": "{4} ({5})"
        },
        "break",
        "colors"
    ]
}
```
