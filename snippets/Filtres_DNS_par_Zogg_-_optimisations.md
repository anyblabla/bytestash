# Filtres DNS par Zogg - optimisations

Le fichier ci-dessous (AdGuardHome.yml) est une amélioration visuelle de l'extrait de code suivant...
- Filtres DNS + règles par Zogg :  https://bytestash.blablalinux.be/s/63f9dbd484fe8e4cb489ab1224f6e688

• adguard
• dns
• règle
• rule
• filter
• pihole
• goaway
• technitium
• yml

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be

# =========================================================================
# === 🛡️ LISTES DE BLOCAGE DNS (BLOCKLISTS) ===
#
# Ces listes sont des collections de domaines bloqués pour le filtrage
# des publicités, du suivi, des malwares et des menaces au niveau DNS.
# Les sources HaGeZi sont réputées pour leur rigueur et leur étendue.
#
# Source de la sélection : Zogg (https://git.zogg.fr/kraoc/agh)
# =========================================================================

filters:
  # -----------------------------------------------------------------------
  # --- LISTES MAJEURES HAGEZI (Multi-usages, Sécurité, Vie Privée) ---
  # -----------------------------------------------------------------------
  - enabled: true
    url: https://cdn.jsdelivr.net/gh/hagezi/dns-blocklists@latest/adblock/ultimate.txt
    name: HaGeZi's Multi Ultimate
    # -> Liste de blocage la plus complète de HaGeZi, couvrant publicité, tracking, malware, crypto, etc.
    id: 1758564499
  - enabled: true
    url: https://cdn.jsdelivr.net/gh/hagezi/dns-blocklists@latest/adblock/tif.txt
    name: HaGeZi's Threat Intelligence Feeds DNS Blocklist
    # -> Concentré sur les flux de renseignements sur les menaces (Malware, C2, etc.). Très axé sécurité.
    id: 1758564513
  - enabled: true
    url: https://cdn.jsdelivr.net/gh/hagezi/dns-blocklists@latest/adblock/doh-vpn-proxy-bypass.txt
    name: HaGeZi's DoH/VPN/TOR/Proxy Bypass
    # -> Bloque les domaines utilisés pour contourner le filtrage DNS (DNS-over-HTTPS, services VPN, TOR).
    id: 1758564504
  - enabled: true
    url: https://cdn.jsdelivr.net/gh/hagezi/dns-blocklists@latest/adblock/hoster.txt
    name: HaGeZi's Badware Hoster blocking
    # -> Bloque les domaines des hébergeurs connus pour diffuser des logiciels malveillants ou du contenu illégal.
    id: 1758564507
  - enabled: true
    url: https://cdn.jsdelivr.net/gh/hagezi/dns-blocklists@latest/adblock/nrd7.txt
    name: HaGeZi's Newly Registered Domains
    # -> Bloque les domaines nouvellement enregistrés (NRD), souvent utilisés par les acteurs de menaces pour des campagnes rapides.
    id: 1758564517
  - enabled: true
    url: https://cdn.jsdelivr.net/gh/hagezi/dns-blocklists@latest/adblock/spam-tlds.txt
    name: HaGeZi's Most Abused TLDs
    # -> Bloque les domaines de premier niveau (TLDs) les plus abusés pour le spam, le phishing ou les malwares.
    id: 1758564519
  - enabled: true
    url: https://cdn.jsdelivr.net/gh/hagezi/dns-blocklists@latest/adblock/urlshortener.txt
    name: HaGeZi's URL Shortener
    # -> Bloque les raccourcisseurs d'URL, souvent abusés pour cacher des destinations malveillantes.
    id: 1758564518
  - enabled: true
    url: https://cdn.jsdelivr.net/gh/hagezi/dns-blocklists@latest/adblock/fake.txt
    name: HaGeZi's Fake
    # -> Domaines de faux services ou de sites d'escroquerie.
    id: 1758564501
  - enabled: true
    url: https://cdn.jsdelivr.net/gh/hagezi/dns-blocklists@latest/adblock/popupads.txt
    name: HaGeZi's Pop-Up Ads
    # -> Domaines connus pour diffuser des publicités pop-up et pop-under agressives.
    id: 1758564502
  - enabled: true
    url: https://cdn.jsdelivr.net/gh/hagezi/dns-blocklists@latest/adblock/spam-tlds-adblock-allow.txt
    name: HaGeZi's Allowlist Referral
    # -> Liste de domaines de référence autorisés. Permet de corriger les problèmes de suivi légitime/référencement.
    id: 1758564556

  # -----------------------------------------------------------------------
  # --- LISTES THÉMATIQUES HAGEZI (Contenu spécifique & Télémétrie) ---
  # -----------------------------------------------------------------------
  - enabled: true
    url: https://cdn.jsdelivr.net/gh/hagezi/dns-blocklists@latest/adblock/anti.piracy.txt
    name: HaGeZi's Anti Piracy
    # -> Bloque les domaines liés aux sites de piratage et de streaming illégal.
    id: 1758564520
  - enabled: true
    url: https://cdn.jsdelivr.net/gh/hagezi/dns-blocklists@latest/adblock/gambling.txt
    name: HaGeZi's Gambling Full
    # -> Bloque les domaines liés aux jeux d'argent, paris et casinos en ligne.
    id: 1758564508
  - enabled: true
    url: https://cdn.jsdelivr.net/gh/hagezi/dns-blocklists@latest/adblock/nsfw.txt
    name: HaGeZi's NSFW
    # -> Bloque les domaines à contenu adulte ou inapproprié (Not Safe For Work).
    id: 1758564527
  - enabled: true
    url: https://cdn.jsdelivr.net/gh/hagezi/dns-blocklists@latest/adblock/native.apple.txt
    name: HaGeZi's Apple Tracker
    # -> Bloque la télémétrie et le suivi spécifiques aux produits et services Apple.
    id: 1758564514
  - enabled: true
    url: https://cdn.jsdelivr.net/gh/hagezi/dns-blocklists@latest/adblock/native.amazon.txt
    name: HaGeZi's Amazon Tracker
    # -> Bloque la télémétrie et le suivi spécifiques aux appareils et services Amazon (Fire TV, Alexa).
    id: 1758564509
  - enabled: true
    url: https://cdn.jsdelivr.net/gh/hagezi/dns-blocklists@latest/adblock/native.samsung.txt
    name: HaGeZi's Samsung Tracker
    # -> Bloque la télémétrie et le suivi spécifiques aux appareils Samsung (Smart TV, téléphones).
    id: 1758564511
  - enabled: true
    url: https://cdn.jsdelivr.net/gh/hagezi/dns-blocklists@latest/adblock/native.winoffice.txt
    name: HaGeZi's Windows/Office Tracker
    # -> Bloque la télémétrie et le suivi spécifiques à Microsoft Windows et Office.
    id: 1758564510
  - enabled: true
    url: https://cdn.jsdelivr.net/gh/hagezi/dns-blocklists@latest/adblock/native.tiktok.extended.txt
    name: HaGeZi's TikTok Tracker Fingerprinting Agressive
    # -> Blocage agressif du suivi et du "fingerprinting" lié à TikTok.
    id: 1758564512

  # -----------------------------------------------------------------------
  # --- LISTES COMPLÉMENTAIRES (Générales et Spécifiques) ---
  # -----------------------------------------------------------------------
  - enabled: true
    url: https://big.oisd.nl/
    name: oisd big
    # -> Liste agrégée massive, très complète pour la publicité, le tracking, et les malwares.
    id: 1758564515
  - enabled: true
    url: https://adguardteam.github.io/AdGuardSDNSFilter/Filters/filter.txt
    name: AdGuard DNS filter
    # -> Filtre DNS par défaut d'AdGuard, optimisé pour la performance.
    id: 1759494022
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_10.txt
    name: Scam Blocklist by DurableNapkin
    # -> Liste de domaines d'escroquerie et d'arnaque.
    id: 1758564521
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_11.txt
    name: Malicious URL Blocklist (URLHaus)
    # -> Bloque les domaines connus pour héberger des malwares (source URLHaus).
    id: 1758564522
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_12.txt
    name: Dandelion Sprout's Anti-Malware List
    # -> Liste anti-malware dédiée.
    id: 1758564523
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_18.txt
    name: Phishing Army
    # -> Liste axée sur le blocage des domaines de phishing.
    id: 1758564529
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_23.txt
    name: WindowsSpyBlocker - Hosts spy rules
    # -> Règle de l'ancienne version de WSB pour bloquer la télémétrie Windows (doublon, mais conservé pour sécurité).
    id: 1758564534
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_24.txt
    name: 1Hosts (Lite) ads, trackers, malware
    # -> Version "Lite" d'une liste populaire, équilibrée pour éviter les faux positifs.
    id: 1758564535
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_30.txt
    name: Phishing URL Blocklist (PhishTank and OpenPhish)
    # -> Blocage des URLs de Phishing provenant de sources fiables.
    id: 1758564541
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_31.txt
    name: Stalkerware Indicators List
    # -> Bloque les indicateurs liés aux logiciels espions ('stalkerware').
    id: 1758564542
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_42.txt
    name: ShadowWhisperer's Malware List
    # -> Liste spécifique de domaines malveillants par ShadowWhisperer.
    id: 1758564553
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_6.txt
    name: Dandelion Sprout's Game Console Adblock List
    # -> Publicités et traqueurs spécifiques aux consoles de jeux.
    id: 1758564569
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_7.txt
    name: Perflyst and Dandelion Sprout's Smart-TV Blocklist
    # -> Télémétrie et publicités spécifiques aux Smart TV.
    id: 1758564570
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_8.txt
    name: NoCoin Filter List
    # -> Bloque les domaines de minage de cryptomonnaie non sollicité.
    id: 1758564571
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_9.txt
    name: The Big List of Hacked Malware Web Sites
    # -> Liste des domaines piratés diffusant des malwares.
    id: 1758564572
  - enabled: true
    url: https://raw.githubusercontent.com/AdguardTeam/FiltersRegistry/master/filters/filter_11_Mobile/filter.txt
    name: Adguard Mobile Advertising
    # -> Règles de blocage pour les publicités spécifiques aux appareils mobiles.
    id: 1758564573
  - enabled: true
    url: https://raw.githubusercontent.com/AdguardTeam/FiltersRegistry/master/filters/filter_14_Annoyances/filter.txt
    name: Adguard Cookies
    # -> Masque les bandeaux et pop-up de consentement aux cookies (ne bloque pas au niveau DNS).
    id: 1758564574
  - enabled: true
    url: https://raw.githubusercontent.com/AdguardTeam/FiltersRegistry/master/filters/filter_16_French/filter.txt
    name: Adguard French Advertising
    # -> Règles de blocage spécifiques aux publicités françaises.
    id: 1758564575
  - enabled: true
    url: https://raw.githubusercontent.com/AdguardTeam/FiltersRegistry/master/filters/filter_17_TrackParam/filter.txt
    name: Adguard General Tracking
    # -> Bloque les paramètres de suivi dans les URLs.
    id: 1758564576
  - enabled: true
    url: https://raw.githubusercontent.com/AdguardTeam/FiltersRegistry/master/filters/filter_2_Base/filter.txt
    name: Adguard Advertising
    # -> Filtre de base contre la publicité.
    id: 1758564577
  - enabled: true
    url: https://raw.githubusercontent.com/AdguardTeam/FiltersRegistry/master/filters/filter_3_Spyware/filter.txt
    name: Adguard JS, CSS, HTML Extensions
    # -> Bloque le code espion et de suivi injecté via JS/CSS/HTML (filtrage de contenu, non DNS).
    id: 1758564578
  - enabled: true
    url: https://raw.githubusercontent.com/AdguardTeam/FiltersRegistry/master/filters/filter_4_Social/filter.txt
    name: Adguard Social Trackers
    # -> Bloque le suivi des réseaux sociaux.
    id: 1758564579
  - enabled: true
    url: https://sebsauvage.net/hosts/raw
    name: sebsauvage
    # -> Liste d'hôtes personnelle par Sebastien Sauvage, axée sur la vie privée et les malwares.
    id: 1758564516
  - enabled: true
    url: https://blocklistproject.github.io/Lists/abuse.txt
    name: The Block List Project Abuse
    # -> Domaines impliqués dans des abus (spam, botnets, etc.).
    id: 1758564596
  - enabled: true
    url: https://blocklistproject.github.io/Lists/ads.txt
    name: The Block List Project Ads
    # -> Publicités.
    id: 1758564597
  - enabled: true
    url: https://blocklistproject.github.io/Lists/drugs.txt
    name: The Block List Project Drugs
    # -> Domaines liés aux drogues.
    id: 1758564598
  - enabled: true
    url: https://blocklistproject.github.io/Lists/fraud.txt
    name: The Block List Project Fraud
    # -> Domaines de fraude.
    id: 1758564599
  - enabled: true
    url: https://blocklistproject.github.io/Lists/gambling.txt
    name: The Block List Project Gambling
    # -> Domaines de jeux d'argent.
    id: 1758564600
  - enabled: true
    url: https://blocklistproject.github.io/Lists/malware.txt
    name: The Block List Project Malware
    # -> Domaines de malwares.
    id: 1758564601
  - enabled: true
    url: https://blocklistproject.github.io/Lists/ransomware.txt
    name: The Block List Project Ransomware
    # -> Domaines liés aux ransomwares.
    id: 1758564602
  - enabled: true
    url: https://blocklistproject.github.io/Lists/scam.txt
    name: The Block List Project Scam
    # -> Domaines d'escroquerie.
    id: 1758564603
  - enabled: true
    url: https://blocklistproject.github.io/Lists/tracking.txt
    name: The Block List Project Tracking
    # -> Domaines de suivi.
    id: 1758564604
  - enabled: true
    url: https://blocklistproject.github.io/Lists/tiktok.txt
    name: The Block List Project TikTok
    # -> Domaines de suivi TikTok.
    id: 1758564605
  - enabled: true
    url: https://raw.githubusercontent.com/matomo-org/referrer-spam-blacklist/master/spammers.txt
    name: Matomo Referrer Spam
    # -> Liste noire des domaines de spam de référence (referrer spam).
    id: 1758564587
  - enabled: true
    url: https://raw.githubusercontent.com/easylist/easylist/refs/heads/master/easylist_cookie/easylist_cookie_general_block.txt
    name: Easylist Cookies
    # -> Règles pour masquer les avis de cookies.
    id: 1758564608
  - enabled: true
    url: https://easylist-downloads.adblockplus.org/antiadblockfilters.txt
    name: Adblock Warning Removal List
    # -> Supprime les messages d'avertissement contre l'utilisation d'Adblock.
    id: 1758564609
  - enabled: true
    url: https://dl.red.flag.domains/red.flag.domains.txt
    name: Red Flag Domains
    # -> Domaines signalés comme dangereux ou à haut risque.
    id: 1758564610
  - enabled: true
    url: https://v.firebog.net/hosts/Prigent-Crypto.txt
    name: Prigent-Crypto
    # -> Domaines liés aux services de cryptomonnaies/minage.
    id: 1758564614
  - enabled: true
    url: https://v.firebog.net/hosts/Prigent-Malware.txt
    name: Prigent-Malware
    # -> Domaines de malwares par Prigent.
    id: 1758564615
  - enabled: true
    url: https://raw.githubusercontent.com/hkamran80/blocklists/refs/heads/main/smart-tv.txt
    name: Smart TV Blocklist
    # -> Télémétrie des Smart TV (liste supplémentaire).
    id: 1758564617
  - enabled: true
    url: https://raw.githubusercontent.com/uBlockOrigin/uAssets/master/filters/filters.txt
    name: uBlock filters
    # -> Filtres généraux uBlock Origin.
    id: 1759494018
  - enabled: true
    url: https://raw.githubusercontent.com/uBlockOrigin/uAssets/master/filters/privacy.txt
    name: uBlock filters – Privacy
    # -> Filtres uBlock Origin axés sur la vie privée.
    id: 1759494019
  - enabled: true
    url: https://raw.githubusercontent.com/uBlockOrigin/uAssets/master/filters/resource-abuse.txt
    name: uBlock filters – Resource abuse
    # -> Filtres uBlock Origin contre l'abus de ressources (minage, etc.).
    id: 1759494020
  - enabled: true
    url: https://raw.githubusercontent.com/uBlockOrigin/uAssets/master/filters/quick-fixes.txt
    name: uBlock filters – Quick fixes
    # -> Correctifs rapides uBlock Origin pour les sites cassés.
    id: 1759494021
  - enabled: true
    url: https://secure.fanboy.co.nz/fanboy-annoyance.txt
    name: Fanboy's Annoyance List
    # -> Bloque les éléments qui gâchent l'expérience utilisateur (bannières, widgets sociaux, pop-ups).
    id: 1759494023
  - enabled: true
    url: https://secure.fanboy.co.nz/fanboy-cookiemonster.txt
    name: Easylist Cookie List
    # -> Complémentaire pour le masquage des avis de cookies.
    id: 1759494024
  - enabled: true
    url: https://raw.githubusercontent.com/crazy-max/WindowsSpyBlocker/master/data/hosts/spy.txt
    name: WindowsSpyBlocker - Hosts spy rules
    # -> Règle WSB contre la télémétrie Windows (version Crazy-Max).
    id: 1759494025
  - enabled: true
    url: https://raw.githubusercontent.com/DandelionSprout/adfilt/master/Alternate%20versions%20Anti-Malware%20List/AntiMalwareAdGuardHome.txt
    name: Dandelion Sprout's Anti-Malware List
    # -> Version AdGuard Home de la liste anti-malware.
    id: 1759494026
  - enabled: true
    url: https://raw.githubusercontent.com/DandelionSprout/adfilt/master/GameConsoleAdblockList.txt
    name: Game Console Adblock List
    # -> Domaines publicitaires pour consoles de jeux.
    id: 1759494027
  - enabled: true
    url: https://raw.githubusercontent.com/durablenapkin/scamblocklist/master/adguard.txt
    name: Scam Blocklist by DurableNapkin
    # -> Liste d'escroquerie et d'arnaque (version AdGuard).
    id: 1759494028
  - enabled: true
    url: https://raw.githubusercontent.com/mitchellkrogza/The-Big-List-of-Hacked-Malware-Web-Sites/master/hosts
    name: The Big List of Hacked Malware Web Sites
    # -> Domaines de sites web piratés.
    id: 1759494029
  - enabled: true
    url: https://raw.githubusercontent.com/nextdns/native-tracking-domains/main/domains/alexa
    name: NextDNS Alexa
    # -> Télémétrie native d'Alexa (NextDNS source).
    id: 1759494030
  - enabled: true
    url: https://raw.githubusercontent.com/nextdns/native-tracking-domains/main/domains/samsung
    name: NextDNS Samsung
    # -> Télémétrie native de Samsung (NextDNS source).
    id: 1759494031
  - enabled: true
    url: https://raw.githubusercontent.com/nextdns/native-tracking-domains/main/domains/windows
    name: NextDNS Windows
    # -> Télémétrie native de Windows (NextDNS source).
    id: 1759494032
  - enabled: true
    url: https://raw.githubusercontent.com/Perflyst/PiHoleBlocklist/master/SmartTV-AGH.txt
    name: Smart-TV Blocklist for AdGuard Home (by Dandelion Sprout)
    # -> Télémétrie/Publicités Smart TV (version AGH).
    id: 1759494033
  - enabled: true
    url: https://raw.githubusercontent.com/PolishFiltersTeam/KADhosts/master/KADhosts.txt
    name: KADhosts
    # -> Liste polonaise de publicités, traqueurs et malwares.
    id: 1759494034
  - enabled: true
    url: https://raw.githubusercontent.com/StevenBlack/hosts/master/alternates/fakenews/hosts
    name: StevenBlack/hosts with the fakenews extension
    # -> Liste StevenBlack incluant une extension pour bloquer les domaines de fausses nouvelles.
    id: 1759494035
  - enabled: true
    url: https://easylist.to/easylist/easylist.txt
    name: EasyList
    # -> Liste de blocage de publicité la plus populaire.
    id: 1759494036
  - enabled: true
    url: https://easylist.to/easylist/easyprivacy.txt
    name: EasyPrivacy
    # -> Liste contre le suivi et la télémétrie.
    id: 1759494037
  - enabled: true
    url: https://filters.adtidy.org/extension/ublock/filters/14.txt
    name: AdGuard Annoyances filter
    # -> Filtre AdGuard contre les éléments gênants.
    id: 1759494038
  - enabled: true
    url: https://gitlab.com/quidsup/notrack-blocklists/raw/master/notrack-malware.txt
    name: NoTrack Malware Blocklist
    # -> Bloque les domaines de malwares de la liste NoTrack.
    id: 1759494039
  - enabled: true
    url: https://malware-filter.gitlab.io/malware-filter/urlhaus-filter-agh.txt
    name: Malicious URL Blocklist (AdGuard Home)
    # -> Blocage d'URLs malveillantes (version AGH).
    id: 1759494040
  - enabled: true
    url: https://www.github.developerdan.com/hosts/lists/dating-services-extended.txt
    name: Lightswitch05's dating-services-extended
    # -> Liste étendue de blocage pour les services de rencontres.
    id: 1759494041
  - enabled: false
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_2.txt
    name: AdAway Default Blocklist (Open-source ad blocker for Android using the hosts file)
    # -> Liste AdAway (désactivée dans votre configuration actuelle, mais conservée).
    id: 2
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_1.txt
    name: AdGuard DNS filter
    # -> Autre lien pour le filtre DNS de base AdGuard (doublon, mais conservé).
    id: 1

# =========================================================================
# === ✍️ REGLES UTILISATEUR (USER_RULES) ===
#
# Règlements personnalisés pour bloquer (BLOCK) ou autoriser (ALLOW)
# des domaines spécifiques qui pourraient ne pas être couverts par les listes
# ou qui cassent certaines applications/services.
#
# Source de la sélection : Zogg (https://git.zogg.fr/kraoc/agh)
# =========================================================================

user_rules:
  # -----------------------------------------------------------------------
  # --- RÈGLES DE BLOCAGE (BLOCK) ---
  # -----------------------------------------------------------------------
  - '#'
  - '# BLOCK - Télémétrie/Publicité Windows 11'
  - '#'
  - '||arc.msn.com^$important' # Publicité/Télémétrie MSN
  - '||ris.api.iris.microsoft.com^$important' # Télémétrie Microsoft RIS
  - '||api.msn.com^$important' # APIs MSN
  - '||assets.msn.com^$important' # Actifs MSN
  - '||c.msn.com^$important' # Services de contenu MSN
  - '||ntp.msn.com^$important' # Service de synchronisation temporelle MSN (peut être lié à la télémétrie)
  - '||srtb.msn.com^$important' # Services de RTB (Real-Time Bidding) publicitaire
  - '||www.msn.com^$important' # Portail MSN
  - '||fd.api.iris.microsoft.com^$important' # Télémétrie IRIS (Flux de données)
  - '||staticview.msn.com^$important' # Contenu statique MSN

  # -----------------------------------------------------------------------
  # --- RÈGLES D'AUTORISATION (ALLOW) ---
  # -----------------------------------------------------------------------
  - '#'
  - '# ALLOW - DNS (Résolveurs Amont & Outils de Vérification)'
  - '#'
  - '@@||dns0.eu^$important'
  - '@@||dns.sb^$important'
  - '@@||dot.sb^$important'
  - '@@||fdn.fr^$important'
  - '@@||cloudflare-dns.com^$important'
  - '@@||one.one.one.one^$important'
  - '@@||dns.quad9.net^$important'
  - '@@||mullvad.net^$important'
  - '@@||dnscheck.tools^$important'
  - '# ALLOW - Sites personnels/Communautaires'
  - '@@||zogg.fr^$important'
  - '@@||blablalinux.be^$important' # CORRECTION: Ajout du '$' manquant pour la compatibilité AdGuard Home
  - '# ALLOW - Infrastructure & Systèmes'
  - '@@||ntp.org^$important' # Serveurs de temps NTP
  - '@@||get.geojs.io^$important' # Service de géolocalisation IP
  - '# ALLOW - Pilotes et Matériel'
  - '@@||events.gfe.nvidia.com^$important' # Mises à jour/Télémétrie de GeForce Experience (souvent requis pour les pilotes)
  - '@@||keychron.fr^$important'
  - '@@||keychron.com^$important'
  - '# ALLOW - Services essentiels Windows 11'
  - '# Certificats'
  - '@@||ctldl.windowsupdate.com^$important' # Téléchargement de listes de confiance des certificats. ESSENTIEL à la sécurité.
  - '@@||ocsp.digicert.com^$important' # Vérification de révocation de certificats (OCSP).
  - '# Devices metadata'
  - '@@||dmd.metaservices.microsoft.com^$important' # Métadonnées des appareils.
  - '# Microsoft account'
  - '@@||login.live.com^$important' # Connexion au compte Microsoft.
  - '# OneDrive'
  - '@@||g.live.com^$important'
  - '@@||onedrive.live.com^$important'
  - '@@||oneclient.sfx.ms^$important'
  - '@@||logincdn.msauth.net^$important'
  - '# Microsoft Defender'
  - '@@||wdcp.microsoft.com^$important' # Envoi de rapports Defender.
  - '@@||smartscreen-prod.microsoft.com^$important'
  - '@@||checkappexec.microsoft.com^$important'
  - '@@||smartscreen.microsoft.com^$important'
  - '@@||ping-edge.smartscreen.microsoft.com^$important'
  - '@@||data-edge.smartscreen.microsoft.com^$important'
  - '@@||nav-edge.smartscreen.microsoft.com^$important' # Vérification de sécurité SmartScreen.
  - '# Microsoft Store'
  - '@@||img-prod-cms-rt-microsoft-com.akamaized.net^$important'
  - '@@||img-s-msn-com.akamaized.net^$important'
  - '@@||storeedgefd.dsx.mp.microsoft.com^$important'
  - '@@||livetileedge.dsx.mp.microsoft.com^$important'
  - '@@||wns.windows.com^$important' # Windows Notification Service.
  - '@@||storecatalogrevocation.storequality.microsoft.com^$important'
  - '@@||displaycatalog.mp.microsoft.com^$important'
  - '# Microsoft Edge'
  - '@@||msedge.api.cdp.microsoft.com^$important' # Service CDP (Client Data Platform) d'Edge.
  - '# Network connection indicator'
  - '@@||www.msftconnecttest.com^$important'
  - '@@||ipv6.msftconnecttest.com^$important' # NCSI (Network Connectivity Status Indicator) pour vérifier l'accès à Internet.
  - '# Settings'
  - '@@||settings-win.data.microsoft.com^$important'
  - '@@||settings.data.microsoft.com^$important'
  - '@@||pipe.aria.microsoft.com^$important' # Télémétrie ARIAPIPE.
  - '# Teams'
  - '@@||config.teams.microsoft.com^$important'
  - '@@||teams.live.com^$important'
  - '@@||teams.events.data.microsoft.com^$important'
  - '@@||statics.teams.cdn.live.net^$important'
  - '@@||teams.events.data.microsoft.com^$important'
  - '# Windows Updates'
  - '@@||definitionupdates.microsoft.com^$important' # Mises à jour des définitions Defender.
  - '@@||prod.do.dsp.mp.microsoft.com^$important'
  - '@@||dl.delivery.mp.microsoft.com^$important'
  - '@@||windowsupdate.com^$important'
  - '@@||delivery.mp.microsoft.com^$important'
  - '@@||update.microsoft.com^$important'
  - '@@||adl.windows.com^$important'
  - '@@||tsfe.trafficshaping.dsp.mp.microsoft.com^$important'
  - '@@||api.cdp.microsoft.com^$important'
  - '# Fonts diffusion'
  - '@@||fs.microsoft.com^$important' # Service de livraison de polices de caractères.
  - '# Licenses'
  - '@@||licensing.mp.microsoft.com^$important' # Vérification des licences logicielles.
  - '# Location'
  - '@@||inference.location.live.net^$important' # Services de localisation Windows.
  - '# Mobile'
  - '@@||mobile.events.data.microsoft.com^$important'
  - '@@||eu-mobile.events.data.microsoft.com^$important' # Services/événements mobiles.
  - '# ALLOW - Sites Divers (Contenu/Services spécifiques)'
  - '@@||l.allocine.fr^$important'
  - '@@||t.notif-colissimo-laposte.info^$important' # Notifications de suivi Colissimo.
  - '@@||image.jeuxvideo.com^$important'
  - '@@||static.jvc.gg^$important'
  - '@@||l.leparisien.fr^$important'
  - '@@||cdn.shopify.com^$important' # Chargement de CDN pour les sites Shopify.
  - '@@||l.20minutes.fr^$important'
  - '@@||cloudimperiumgames.com^$important' # Star Citizen (Cloud Imperium Games).
  - '@@||robertsspaceindustries.com^$important'
  - '@@||erkul.games^$important'
  - '@@||finder.cstone.space^$important'
  - '@@||cdprojekt.red^$important' # CD Projekt RED (The Witcher, Cyberpunk).
  - '# ALLOW - Réseaux Sociaux & Plateformes'
  - '# Tiktok (Si nécessaire pour le fonctionnement)'
  - '@@||www.tiktok.com^$important'
  - '@@||m.tiktok.com^$important'
  - '@@||tiktokw.eu^$important'
  - '@@||tiktokv.com^$important'
  - '@@||tiktokcdn.com^$important'
  - '@@||tiktokcdn-eu.com^$important'
  - '# Twitter (X)'
  - '@@||t.co^$important' # Service de raccourcissement de liens de Twitter/X.
  - '# YouTube'
  - '@@||googlevideo.com^$important' # Serveur de streaming vidéo de YouTube (essentiel pour regarder des vidéos).
  - '# Discord'
  - '@@||cdn.discordapp.com^$important' # CDN (Content Delivery Network) pour les médias/actifs Discord.
```
