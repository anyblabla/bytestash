# DNS - Liste blanche complète pour les serveurs DNS en amont

Si vous utilisez des listes restrictives dans votre solution réseau qui bloque les publicités et les traqueurs (AdGuard Home, PiHole, GoAway, Technitium, etc.), il ne faut pas oublier de laisser passer les principales adresses des serveurs DNS en amont.

• dns
• upstream
• adguard
• pihole
• goaway
• technitium

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
#
# ! DNS - Complete Whitelist for Upstream DNS Servers
# !! DNS0.EU and DNS.SB Services
@@||dns0.eu^$important
@@||dns.sb^$important
@@||dot.sb^$important
# !! DNS Verification Tool
@@||dnscheck.tools^$important
# !! Cloudflare Entries (1.1.1.1)
@@||cloudflare-dns.com^$important
@@||one.one.one.one^$important
@@||1dot1dot1dot1.cloudflare-dns.com^$important
# !! Quad9 Entries (9.9.9.9)
@@||dns.quad9.net^$important
@@||dns10.quad9.net^$important
# !! AdGuard DNS Entries
@@||dns-unfiltered.adguard.com^$important
@@||dns.adguard-dns.com^$important
@@||dns-family.adguard.com^$important
@@||dns-adult.adguard.com^$important
# !! FDN and Mullvad Entries
@@||fdn.fr^$important
@@||mullvad.net^$important
# !! OpenDNS Entries (Cisco)
@@||dns.opendns.com^$important
@@||doh.opendns.com^$important
# !! Google Public DNS Entries (8.8.8.8)
@@||dns.google^$important
@@||dns-over-https.google.com^$important
# !! Entry for Root Servers
@@||root-servers.net^$important
# !! Generic fallback entry for IP-based DNS resolution
@@||use-application-dns.net^$important
```

```plaintext
Il y a en fait un intérêt crucial à inclure Cloudflare et Google (et tous les autres serveurs de ma liste) dans la partie ALLOW de mes filtres :

1. Le rôle de la liste blanche ALLOW
Ma liste n'autorise pas ces domaines pour la navigation web quotidienne ; elle autorise ces domaines pour AdGuard Home lui-même :

AdGuard Home n'est pas un résolveur DNS final. C'est un filtre.

Il reçoit une requête (ex: google.com) depuis mon téléphone, vérifie toutes les listes de blocage...

... et si la requête est autorisée, il doit l'envoyer à un serveur DNS externe pour obtenir la vraie adresse IP.

C'est là qu'interviennent Cloudflare (1.1.1.1), Google (8.8.8.8), Quad9, AdGuard DNS, etc. Ce sont mes serveurs en amont (Upstream DNS) que j'ai configurés dans AdGuard Home.

Si je ne les autorise pas (avec la règle @@||...^$important), ma liste de blocage ultra-restrictive pourrait bloquer accidentellement la communication de mon propre AdGuard Home avec ces serveurs externes. Résultat : plus aucune navigation Internet !

2. Le choix de Cloudflare, Google, etc. (Performance et redondance)
Je choisis d'utiliser plusieurs de ces serveurs DNS publics (et non un seul) pour deux raisons principales :

Redondance et Fiabilité : Si un serveur (ex: Quad9) a une panne temporaire ou est lent, AdGuard Home bascule immédiatement vers un autre (ex: Cloudflare ou Google). Cela garantit que la résolution DNS ne s'arrête jamais.

Performance : J'utilise les serveurs publics les plus rapides et les plus fiables du marché (DoT ou DoH) pour garantir des temps de résolution minimum.

En résumé, la liste d'autorisation ne sert pas à 'laisser passer' ces sites aux utilisateurs, mais à garantir que l'outil de filtrage (AdGuard Home) puisse communiquer avec les outils de résolution (les serveurs DNS externes) sans être bloqué par ses propres règles.
```
