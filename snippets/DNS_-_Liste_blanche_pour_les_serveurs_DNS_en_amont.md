# DNS - Liste blanche pour les serveurs DNS en amont

Si vous utilisez des listes restrictives dans votre solution réseau qui bloque les publicités et les traqueurs (AdGuard Home, PiHole, GoAway, Technitium, etc.), il ne faut pas oublier de laisser passer les principales adresses des serveurs DNS en amont.

• dns
• upstream
• adguard
• pihole
• goaway
• technitium

```yaml
user_rules:
### REGLES D'AUTORISATION (ALLOW) ###
#
#
- '### DNS & INFRASTRUCTURE ###'
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
#
#
  - '## DNS - Liste Blanche Complète pour Serveurs Amont'
  - '# DNS0.EU and DNS.SB Services'
  - '@@||dns0.eu^$important'
  - '@@||dns.sb^$important'
  - '@@||dot.sb^$important'
  - '# DNS Verification Tool'
  - '@@||dnscheck.tools^$important'
  - '# Cloudflare Entries (1.1.1.1)'
  - '@@||cloudflare-dns.com^$important'
  - '@@||one.one.one.one^$important'
  - '@@||1dot1dot1dot1.cloudflare-dns.com^$important'
  - '# Quad9 Entries (9.9.9.9)'
  - '@@||dns.quad9.net^$important'
  - '@@||dns10.quad9.net^$important'
  - '# AdGuard DNS Entries'
  - '@@||dns-unfiltered.adguard.com^$important'
  - '@@||dns.adguard-dns.com^$important'
  - '@@||dns-family.adguard.com^$important'
  - '@@||dns-adult.adguard.com^$important'
  - '# FDN and Mullvad Entries'
  - '@@||fdn.fr^$important'
  - '@@||mullvad.net^$important'
  - '# OpenDNS Entries (Cisco)'
  - '@@||dns.opendns.com^$important'
  - '@@||doh.opendns.com^$important'
  - '# Google Public DNS Entries (8.8.8.8)'
  - '@@||dns.google^$important'
  - '@@||dns-over-https.google.com^$important'
  - '# Entry for Root Servers'
  - '@@||root-servers.net^$important'
  - '# Generic fallback entry for IP-based DNS resolution'
  - '@@||use-application-dns.net^$important'
```

```plaintext
====================================================================
EXPLICATION DES REGLES D'AUTORISATION (ALLOW) DANS ADGUARD HOME
====================================================================

NOTE : Les regles d'autorisation (ALLOW) pour Cloudflare, Google, Quad9, etc., 
ne servent pas a autoriser la navigation web des utilisateurs vers ces domaines, 
mais a garantir le fonctionnement interne d'AdGuard Home lui-meme.

1. ROLE DU FILTRE DNS ADGUARD HOME

AdGuard Home (AGH) est un filtre DNS, pas le serveur de resolution final. 
Lorsqu'il recoit une requete (ex: google.com), il la filtre par rapport a ses 
listes de blocage. Si la requete est autorisee (non bloquee), AGH doit ensuite 
l'envoyer a un serveur DNS externe (Upstream DNS) pour obtenir l'adresse IP.

2. NECESSITE DE LA LISTE BLANCHE (ALLOW)

Ces serveurs Upstream (Cloudflare 1.1.1.1, Google 8.8.8.8, Quad9, etc.) sont 
configures dans AGH comme etant les serveurs de confiance.

Si ces domaines (ex: dns.google) ne figurent pas dans la liste d'autorisation 
(regle @@||...^$important), la liste de blocage ultra-restrictive d'AGH 
pourrait accidentellement bloquer sa propre communication avec ces serveurs externes.

Resultat : la resolution DNS s'arreterait, causant une panne totale d'Internet.

3. REDONDANCE ET PERFORMANCE

L'utilisation de plusieurs serveurs DNS publics est une strategie pour la 
fiabilite :
- Redondance : Si un serveur (ex: Quad9) est lent ou tombe en panne, AGH bascule 
  immediatement vers un autre (ex: Cloudflare ou Google).
- Performance : Cela garantit des temps de resolution DNS minimaux.

En resume, l'autorisation garantit la stabilite du service : elle permet a 
l'outil de filtrage (AdGuard Home) de communiquer avec les outils de resolution 
(les serveurs DNS externes) sans etre bloque par ses propres regles.
```
