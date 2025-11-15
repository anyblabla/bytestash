# Filtres DNS

Filtres DNS par URL pour les solutions de blocage telles que AdGuard Home, PiHole, GoAway, Technitium, etc.

• dns
• adguard
• pihole
• goaway
• filtre
• filter
• technitium

```plaintext
# Modifications apportées par Blabla Linux : https://link.blablalinux.be

# =========================================================================
# === 🛡️ LISTES DE BLOCAGE DNS (BLOCKLISTS) ===
#
# Ces listes sont utilisées pour bloquer les publicités, les traqueurs,
# les malwares et les contenus indésirables au niveau DNS.
#
# =========================================================================

### FILTRES GÉNÉRAUX & MAJEURS (AD/TRACKING/CORE) ###
#
url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_27.txt
name: OISD Blocklist Full
# -> Une liste agrégée, très complète, couvrant publicité, tracking, malware, phishing et contenu abusif.

url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_1.txt
name: AdGuard DNS filter
# -> Filtre principal par AdGuard, optimisé pour le blocage DNS.

url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_2.txt
name: AdAway Default Blocklist
# -> Liste populaire issue de la communauté Android/Linux pour le blocage d'hôtes publicitaires.

url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_4.txt
name: Dan Pollock's List
# -> Liste historique et très respectée, axée principalement sur la publicité.

url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_33.txt
name: Steven Black's List
# -> Liste agrégée bien connue, regroupant plusieurs sources.

url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_3.txt
name: Peter Lowe's Blocklist
# -> Concentrée sur le blocage des serveurs publicitaires et de suivi.

url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_53.txt
name: AWAvenue Ads Rule
# -> Règle spécifique pour le blocage de publicités.

#
#
### FILTRES SÉCURITÉ & MALWARE (MENACES) ###
#
url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_49.txt
name: HaGeZi's Ultimate Blocklist
# -> Liste HaGeZi très étendue couvrant Adware, Tracking, Malware, Phishing, etc.

url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_44.txt
name: HaGeZi's Threat Intelligence Feeds
# -> Flux de renseignements sur les menaces (Malware, C2, etc.).

url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_12.txt
name: Dandelion Sprout's Anti-Malware List
# -> Liste ciblée contre les logiciels malveillants.

url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_50.txt
name: uBlock₀ filters – Badware risks
# -> Filtres uBlock₀ axés sur les sites à risque/malveillants.

url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_30.txt
name: Phishing URL Blocklist (PhishTank and OpenPhish)
# -> Blocage des URLs de Phishing provenant de sources reconnues.

url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_18.txt
name: Phishing Army
# -> Liste dédiée aux domaines de Phishing.

url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_11.txt
name: Malicious URL Blocklist (URLHaus)
# -> Blocage des domaines connus pour héberger des logiciels malveillants.

url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_9.txt
name: The Big List of Hacked Malware Web Sites
# -> Liste des sites web piratés diffusant des malwares.

url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_42.txt
name: ShadowWhisperer's Malware List
# -> Liste de domaines de malware par ShadowWhisperer.

url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_55.txt
name: HaGeZi's Badware Hoster Blocklist
# -> Blocage des hôtes connus pour distribuer des logiciels malveillants.

url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_56.txt
name: HaGeZi's The World's Most Abused TLDs
# -> Blocage des domaines de premier niveau (TLDs) les plus souvent abusés pour le spam/malware.

#
#
### FILTRES ANTI-TRACKING & VIE PRIVÉE (TÉLÉMÉTRIE) ###
#
url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_32.txt
name: The NoTracking blocklist
# -> Liste axée sur le blocage des mécanismes de suivi et de télémétrie.

url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_69.txt
name: ShadowWhisperer Tracking List
# -> Liste ciblée contre les traqueurs par ShadowWhisperer.

url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_31.txt
name: Stalkerware Indicators List
# -> Blocage des indicateurs liés aux logiciels espions (stalkerware).

url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_63.txt
name: HaGeZi's Windows/Office Tracker Blocklist
# -> Bloque les domaines de suivi spécifiques à Microsoft Windows et Office.

url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_67.txt
name: HaGeZi's Apple Tracker Blocklist
# -> Bloque les domaines de suivi spécifiques aux appareils et services Apple.

url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_61.txt
name: HaGeZi's Samsung Tracker Blocklist
# -> Bloque les domaines de suivi spécifiques aux appareils Samsung.

url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_66.txt
name: HaGeZi's OPPO & Realme Tracker Blocklist
# -> Bloque les domaines de suivi spécifiques aux appareils OPPO et Realme.

url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_65.txt
name: HaGeZi's Vivo Tracker Blocklist
# -> Bloque les domaines de suivi spécifiques aux appareils Vivo.

url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_60.txt
name: HaGeZi's Xiaomi Tracker Blocklist
# -> Bloque les domaines de suivi spécifiques aux appareils Xiaomi.

url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_37.txt
name: No Google
# -> Liste bloquant les domaines de Google et de suivi associés (peut casser certaines fonctionnalités Google).

#
#
### FILTRES SPÉCIFIQUES & THÉMATIQUES ###
#
url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_52.txt
name: HaGeZi's Encrypted DNS/VPN/TOR/Proxy Bypass
# -> Tente de bloquer les domaines permettant de contourner les restrictions DNS (DoH, TOR, VPN).

url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_71.txt
name: HaGeZi's DNS Rebind Protection
# -> Protection contre les attaques de rebinding DNS sur votre réseau local.

url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_70.txt
name: 1Hosts (Xtra)
# -> Liste très étendue pour publicités et traqueurs.

url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_8.txt
name: NoCoin Filter List
# -> Blocage des domaines liés au minage de cryptomonnaie non sollicité dans les navigateurs.

url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_10.txt
name: Scam Blocklist by DurableNapkin
# -> Liste pour bloquer les domaines d'escroquerie et d'arnaque.

url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_59.txt
name: AdGuard DNS Popup Hosts filter
# -> Filtre pour les hôtes qui servent des popups non sollicités.

url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_39.txt
name: Dandelion Sprout's Anti Push Notifications
# -> Tente de bloquer les domaines de serveurs de notifications push abusives.

url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_6.txt
name: Dandelion Sprout's Game Console Adblock List
# -> Publicités spécifiques aux consoles de jeux (Xbox, PlayStation, etc.).

url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_7.txt
name: Perflyst and Dandelion Sprout's Smart-TV Blocklist
# -> Télémétrie et publicités sur les Smart TV.

url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_47.txt
name: HaGeZi's Gambling Blocklist
# -> Bloque les domaines liés aux jeux d'argent et aux paris.

url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_46.txt
name: HaGeZi's Anti-Piracy Blocklist
# -> Bloque les domaines liés à la piraterie (sites de streaming illégal, torrents, etc.).

url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_57.txt
name: ShadowWhisperer's Dating List
# -> Bloque les domaines liés aux sites de rencontres.

url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_54.txt
name: HaGeZi's DynDNS Blocklist
# -> Blocage des domaines de services DynDNS souvent utilisés par des acteurs malveillants.

url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_68.txt
name: HaGeZi's URL Shortener Blocklist
# -> Bloque les raccourcisseurs d'URL qui peuvent cacher des menaces.

url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_45.txt
name: HaGeZi's Allowlist Referral
# -> Domaine utilisé pour empêcher l'usurpation d'identité/les fausses références dans les journaux.

url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_62.txt
name: Ukrainian Security Filter
# -> Filtre orienté sécurité par une source ukrainienne.

#
#
### FILTRES RÉGIONAUX & LINGUISTIQUES (PAYS) ###
#
url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_21.txt
name: 'CHN: anti-AD'
# -> Spécifique à la Chine pour la publicité.

url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_29.txt
name: 'CHN: AdRules DNS List'
# -> Règles publicitaires spécifiques à la Chine.

url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_35.txt
name: 'HUN: Hufilter'
# -> Filtres spécifiques à la Hongrie.

url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_22.txt
name: 'IDN: ABPindo'
# -> Filtres spécifiques à l'Indonésie.

url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_19.txt
name: 'IRN: PersianBlocker list'
# -> Filtres spécifiques à l'Iran.

url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_25.txt
name: 'KOR: List-KR DNS'
# -> Filtres spécifiques à la Corée.

url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_43.txt
name: 'ISR: EasyList Hebrew'
# -> Filtres spécifiques à Israël (langue hébraïque).

url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_15.txt
name: 'KOR: YousList'
# -> Filtres coréens supplémentaires.

url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_36.txt
name: 'LIT: EasyList Lithuania'
# -> Filtres spécifiques à la Lituanie.

url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_20.txt
name: 'MKD: Macedonian Pi-hole Blocklist'
# -> Filtres spécifiques à la Macédoine.

url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_13.txt
name: 'NOR: Dandelion Sprouts nordiske filtre'
# -> Filtres spécifiques aux pays nordiques.

url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_41.txt
name: 'POL: CERT Polska List of malicious domains'
# -> Liste de domaines malveillants par CERT Pologne.

url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_14.txt
name: 'POL: Polish filters for Pi-hole'
# -> Filtres polonais pour Pi-hole (compatible DNS).

url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_17.txt
name: 'SWE: Frellwit''s Swedish Hosts File'
# -> Filtres spécifiques à la Suède.

url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_26.txt
name: 'TUR: turk-adlist'
# -> Filtres spécifiques à la Turquie.

url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_40.txt
name: 'TUR: Turkish Ad Hosts'
# -> Hôtes publicitaires spécifiques à la Turquie.

url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_16.txt
name: 'VNM: ABPVN List'
# -> Filtres spécifiques au Vietnam.
```

```yaml
#
# =========================================================================
# === 🛡️ LISTES DE BLOCAGE DNS (BLOCKLISTS) ===
#
# Ces listes sont utilisées pour bloquer les publicités, les traqueurs,
# les malwares et les contenus indésirables au niveau DNS.
#
# =========================================================================
filters:
  # -----------------------------------------------------------------------
  # --- FILTRES GÉNÉRAUX & MAJEURS (AD/TRACKING/CORE) ---
  # -----------------------------------------------------------------------
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_27.txt
    name: OISD Blocklist Full
    # -> Liste agrégée, très complète, couvrant publicité, tracking, malware, phishing et contenu abusif.
    id: 1677633577
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_1.txt
    name: AdGuard DNS filter
    # -> Filtre principal par AdGuard, optimisé pour le blocage DNS.
    id: 1
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_2.txt
    name: AdAway Default Blocklist
    # -> Liste populaire issue de la communauté Android/Linux pour le blocage d'hôtes publicitaires.
    id: 2
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_4.txt
    name: Dan Pollock's List
    # -> Liste historique et très respectée, axée principalement sur la publicité.
    id: 1675374098
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_33.txt
    name: Steven Black's List
    # -> Liste agrégée bien connue, regroupant plusieurs sources de blocage général.
    id: 1675374103
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_3.txt
    name: Peter Lowe's Blocklist
    # -> Concentrée sur le blocage des serveurs publicitaires et de suivi.
    id: 1675374102
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_53.txt
    name: AWAvenue Ads Rule
    # -> Règle spécifique pour le blocage de publicités.
    id: 1733947719

  # -----------------------------------------------------------------------
  # --- FILTRES SÉCURITÉ & MALWARE (MENACES) ---
  # -----------------------------------------------------------------------
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_49.txt
    name: HaGeZi's Ultimate Blocklist
    # -> Liste HaGeZi très étendue couvrant Adware, Tracking, Malware, Phishing, etc.
    id: 1709865126
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_44.txt
    name: HaGeZi's Threat Intelligence Feeds
    # -> Flux de renseignements sur les menaces (Malware, C2, etc.).
    id: 1709921344
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_12.txt
    name: Dandelion Sprout's Anti-Malware List
    # -> Liste ciblée contre les logiciels malveillants.
    id: 1675374106
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_50.txt
    name: uBlock₀ filters – Badware risks
    # -> Filtres uBlock₀ axés sur les sites à risque/malveillants.
    id: 1709921343
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_30.txt
    name: Phishing URL Blocklist (PhishTank and OpenPhish)
    # -> Blocage des URLs de Phishing provenant de sources reconnues.
    id: 1675374105
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_18.txt
    name: Phishing Army
    # -> Liste dédiée aux domaines de Phishing.
    id: 1709921345
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_11.txt
    name: Malicious URL Blocklist (URLHaus)
    # -> Blocage des domaines connus pour héberger des logiciels malveillants.
    id: 1675374109
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_9.txt
    name: The Big List of Hacked Malware Web Sites
    # -> Liste des sites web piratés diffusant des malwares.
    id: 1675374110
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_42.txt
    name: ShadowWhisperer's Malware List
    # -> Liste de domaines de malware par ShadowWhisperer.
    id: 1709921346
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_55.txt
    name: HaGeZi's Badware Hoster Blocklist
    # -> Blocage des hôtes connus pour distribuer des logiciels malveillants.
    id: 1720658592
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_56.txt
    name: HaGeZi's The World's Most Abused TLDs
    # -> Blocage des domaines de premier niveau (TLDs) les plus souvent abusés pour le spam/malware.
    id: 1720658593

  # -----------------------------------------------------------------------
  # --- FILTRES ANTI-TRACKING & VIE PRIVÉE (TÉLÉMÉTRIE) ---
  # -----------------------------------------------------------------------
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_32.txt
    name: The NoTracking blocklist
    # -> Liste axée sur le blocage des mécanismes de suivi et de télémétrie.
    id: 1675374101
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_69.txt
    name: ShadowWhisperer Tracking List
    # -> Liste ciblée contre les traqueurs par ShadowWhisperer.
    id: 1759866874
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_31.txt
    name: Stalkerware Indicators List
    # -> Blocage des indicateurs liés aux logiciels espions (stalkerware).
    id: 1675374108
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_23.txt
    name: WindowsSpyBlocker - Hosts spy rules
    # -> Blocage des domaines de télémétrie et d'espionnage de Windows.
    id: 1675374104
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_63.txt
    name: HaGeZi's Windows/Office Tracker Blocklist
    # -> Bloque les domaines de suivi spécifiques à Microsoft Windows et Office.
    id: 1733947718
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_67.txt
    name: HaGeZi's Apple Tracker Blocklist
    # -> Bloque les domaines de suivi spécifiques aux appareils et services Apple.
    id: 1759866875
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_61.txt
    name: HaGeZi's Samsung Tracker Blocklist
    # -> Bloque les domaines de suivi spécifiques aux appareils Samsung.
    id: 1759866877
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_66.txt
    name: HaGeZi's OPPO & Realme Tracker Blocklist
    # -> Bloque les domaines de suivi spécifiques aux appareils OPPO et Realme.
    id: 1759866876
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_65.txt
    name: HaGeZi's Vivo Tracker Blocklist
    # -> Bloque les domaines de suivi spécifiques aux appareils Vivo.
    id: 1759866878
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_60.txt
    name: HaGeZi's Xiaomi Tracker Blocklist
    # -> Bloque les domaines de suivi spécifiques aux appareils Xiaomi.
    id: 1759866879
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_37.txt
    name: No Google
    # -> Liste bloquant les domaines de Google et de suivi associés (peut casser certaines fonctionnalités Google).
    id: 1759866880

  # -----------------------------------------------------------------------
  # --- FILTRES SPÉCIFIQUES & THÉMATIQUES (LOISIRS, OUTILS) ---
  # -----------------------------------------------------------------------
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_52.txt
    name: HaGeZi's Encrypted DNS/VPN/TOR/Proxy Bypass
    # -> Tente de bloquer les domaines permettant de contourner les restrictions DNS (DoH, TOR, VPN).
    id: 1709921342
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_71.txt
    name: HaGeZi's DNS Rebind Protection
    # -> Protection contre les attaques de rebinding DNS sur votre réseau local.
    id: 1759235962
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_70.txt
    name: 1Hosts (Xtra)
    # -> Liste très étendue pour publicités et traqueurs.
    id: 1759866873
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_8.txt
    name: NoCoin Filter List
    # -> Blocage des domaines liés au minage de cryptomonnaie non sollicité dans les navigateurs.
    id: 1677633575
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_10.txt
    name: Scam Blocklist by DurableNapkin
    # -> Liste pour bloquer les domaines d'escroquerie et d'arnaque.
    id: 1675374107
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_59.txt
    name: AdGuard DNS Popup Hosts filter
    # -> Filtre pour les hôtes qui servent des popups non sollicités.
    id: 1720658585
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_39.txt
    name: Dandelion Sprout's Anti Push Notifications
    # -> Tente de bloquer les domaines de serveurs de notifications push abusives.
    id: 1720658589
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_6.txt
    name: Dandelion Sprout's Game Console Adblock List
    # -> Publicités spécifiques aux consoles de jeux (Xbox, PlayStation, etc.).
    id: 1722234149
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_7.txt
    name: Perflyst and Dandelion Sprout's Smart-TV Blocklist
    # -> Télémétrie et publicités sur les Smart TV.
    id: 1722234150
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_47.txt
    name: HaGeZi's Gambling Blocklist
    # -> Bloque les domaines liés aux jeux d'argent et aux paris.
    id: 1720658590
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_46.txt
    name: HaGeZi's Anti-Piracy Blocklist
    # -> Bloque les domaines liés à la piraterie (sites de streaming illégal, torrents, etc.).
    id: 1720658587
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_57.txt
    name: ShadowWhisperer's Dating List
    # -> Bloque les domaines liés aux sites de rencontres.
    id: 1720658591
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_54.txt
    name: HaGeZi's DynDNS Blocklist
    # -> Blocage des domaines de services DynDNS souvent utilisés par des acteurs malveillants.
    id: 1709921341
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_68.txt
    name: HaGeZi's URL Shortener Blocklist
    # -> Bloque les raccourcisseurs d'URL qui peuvent cacher des menaces.
    id: 1759235963
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_45.txt
    name: HaGeZi's Allowlist Referral
    # -> Domaine utilisé pour empêcher l'usurpation d'identité/les fausses références dans les journaux.
    id: 1720658586
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_62.txt
    name: Ukrainian Security Filter
    # -> Filtre orienté sécurité par une source ukrainienne.
    id: 1759866881

  # -----------------------------------------------------------------------
  # --- FILTRES RÉGIONAUX & LINGUISTIQUES (PAYS) ---
  # -----------------------------------------------------------------------
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_21.txt
    name: 'CHN: anti-AD'
    # -> Publicités spécifiques à la Chine.
    id: 1759866882
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_29.txt
    name: 'CHN: AdRules DNS List'
    # -> Règles publicitaires spécifiques à la Chine.
    id: 1759866883
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_35.txt
    name: 'HUN: Hufilter'
    # -> Filtres spécifiques à la Hongrie.
    id: 1759866884
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_22.txt
    name: 'IDN: ABPindo'
    # -> Filtres spécifiques à l'Indonésie.
    id: 1759866885
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_19.txt
    name: 'IRN: PersianBlocker list'
    # -> Filtres spécifiques à l'Iran.
    id: 1759866886
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_25.txt
    name: 'KOR: List-KR DNS'
    # -> Filtres spécifiques à la Corée.
    id: 1759866887
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_43.txt
    name: 'ISR: EasyList Hebrew'
    # -> Filtres spécifiques à Israël (langue hébraïque).
    id: 1759866888
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_15.txt
    name: 'KOR: YousList'
    # -> Filtres coréens supplémentaires.
    id: 1759866889
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_36.txt
    name: 'LIT: EasyList Lithuania'
    # -> Filtres spécifiques à la Lituanie.
    id: 1759866890
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_20.txt
    name: 'MKD: Macedonian Pi-hole Blocklist'
    # -> Filtres spécifiques à la Macédoine.
    id: 1759866891
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_13.txt
    name: 'NOR: Dandelion Sprouts nordiske filtre'
    # -> Filtres spécifiques aux pays nordiques.
    id: 1759866892
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_41.txt
    name: 'POL: CERT Polska List of malicious domains'
    # -> Liste de domaines malveillants par CERT Pologne.
    id: 1759866893
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_14.txt
    name: 'POL: Polish filters for Pi-hole'
    # -> Filtres polonais pour Pi-hole (compatible DNS).
    id: 1759866894
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_17.txt
    name: 'SWE: Frellwit''s Swedish Hosts File'
    # -> Filtres spécifiques à la Suède.
    id: 1759866895
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_26.txt
    name: 'TUR: turk-adlist'
    # -> Filtres spécifiques à la Turquie.
    id: 1759866896
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_40.txt
    name: 'TUR: Turkish Ad Hosts'
    # -> Hôtes publicitaires spécifiques à la Turquie.
    id: 1759866897
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_16.txt
    name: 'VNM: ABPVN List'
    # -> Filtres spécifiques au Vietnam.
    id: 1759866898
```
