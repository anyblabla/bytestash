# Fastfetch Minimal - config.jsonc

Une configuration Fastfetch ultra-minimaliste baptisée "The Thin Line". Conçue pour les utilisateurs intensifs de terminaux divisés (split screen), elle privilégie la verticalité et la sobriété pour ne pas empiéter sur votre espace de travail.

Points forts :
- Encombrement Minimum : Structure verticale utilisant des bordures fines pour une lecture rapide sans largeur excessive.
- Design Épuré : Utilisation de couleurs sobres et d'icônes discrètes pour une intégration parfaite dans n'importe quel environnement.
- Optimisation Espace : Idéal pour les configurations en "Tiling Window Manager" ou les multiplexeurs comme Tmux et Terminator.
- Lisibilité : Les informations essentielles (CPU, RAM, IP) restent accessibles d'un seul coup d'œil.

Usage :
- Copier le contenu dans ~/.config/fastfetch/thin-line.jsonc
- Lancer avec la commande : fastfetch -c ~/.config/fastfetch/thin-line.jsonc

• fastfetch
• fetch
• jsonc
• pve

```json
// # Modifications apportées par Blabla Linux : https://link.blablalinux.be
{
    "$schema": "https://github.com/fastfetch-cli/fastfetch/raw/dev/doc/json_schema.json",
    "logo": {
        "source": "debian_small",
        "padding": {
            "top": 1,
            "left": 2
        },
        "color": {
            "1": "white"
        }
    },
    "display": {
        "separator": " ",
        "color": {
            "keys": "magenta"
        }
    },
    "modules": [
        {
            "type": "title",
            "format": "{1}@{2}"
        },
        {
            "type": "custom",
            "format": " \u001b[90m│\u001b[0m"
        },
        {
            "type": "os",
            "key": " \u001b[90m│\u001b[0m 🐧"
        },
        {
            "type": "kernel",
            "key": " \u001b[90m│\u001b[0m ⚙️ "
        },
        {
            "type": "uptime",
            "key": " \u001b[90m│\u001b[0m ⏱️ "
        },
        {
            "type": "cpu",
            "key": " \u001b[90m│\u001b[0m 🧠",
            "temp": true
        },
        {
            "type": "memory",
            "key": " \u001b[90m│\u001b[0m 💾"
        },
        {
            "type": "localip",
            "key": " \u001b[90m│\u001b[0m 🌐",
            "showIpv6": false
        },
        {
            "type": "custom",
            "format": " \u001b[90m╰───────\u001b[0m"
        }
    ]
}
```
