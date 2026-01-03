# Fastfetch Emoji - config.jsonc

Configuration Fastfetch "Modern Emoji Edition" (Debian/Ubuntu/Mint)

Une version visuelle et moderne de Fastfetch utilisant des émojis pour une identification rapide des composants système. 

Points forts :
- Iconographie intuitive : Utilisation d'émojis UTF-8 pour chaque module.
- Diagnostic complet : Inclut température CPU, IP locale, batterie et état de l'alimentation.
- Mise en page soignée : Alignement vertical optimisé pour les polices modernes.
- Universel : Adapté pour PC portables et fixes sous environnements Debian-based.

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
        "separator": " ➜  "
    },
    "modules": [
        "title",
        "separator",
        {
            "type": "os",
            "key": "🐧 OS          "
        },
        {
            "type": "host",
            "key": "💻 Machine     "
        },
        {
            "type": "kernel",
            "key": "⚙️  Kernel      "
        },
        {
            "type": "uptime",
            "key": "⏱️  Uptime      "
        },
        {
            "type": "packages",
            "key": "📦 Packages    "
        },
        {
            "type": "shell",
            "key": "🐚 Shell       "
        },
        {
            "type": "display",
            "key": "🖥️  Display     "
        },
        {
            "type": "de",
            "key": "🏠 DE          "
        },
        {
            "type": "wm",
            "key": "🪟 WM          "
        },
        {
            "type": "terminal",
            "key": "⌨️  Terminal    "
        },
        {
            "type": "cpu",
            "key": "🧠 CPU         ",
            "temp": true
        },
        {
            "type": "gpu",
            "key": "🎮 GPU         ",
            "hideType": "all",
            "format": "{1} {2}"
        },
        {
            "type": "memory",
            "key": "💾 Memory      "
        },
        {
            "type": "disk",
            "key": "💽 Disk        "
        },
        {
            "type": "localip",
            "key": "🌐 Local IP    ",
            "showIpv6": false
        },
        "break",
        {
            "type": "battery",
            "key": "🔋 Battery     "
        },
        {
            "type": "poweradapter",
            "key": "🔌 Power       "
        },
        "break",
        "colors"
    ]
}
```
