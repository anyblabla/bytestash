# Filtres DNS par Zogg

Filtres de blocage DNS pour les solutions de blocage telles que AdGuard Home, PiHole, GoAway, Technitium, etc. Est fourni avec un ensemble de règles de filtrage personnalisées.
Les filtres et les règles sont fournis par  Zogg :
- https://zogg.fr
- https://git.zogg.fr/kraoc/agh

• dns
• adguard
• pihole
• goaway
• filtre
• filter
• règle
• rule
• zogg

```yaml
# Les filtres de blocage DNS sont fournis par  Zogg :
# - https://zogg.fr
# - https://git.zogg.fr/kraoc/agh

# Replace "filters" entry with these :p

filters:
  - enabled: true
    url: https://cdn.jsdelivr.net/gh/hagezi/dns-blocklists@latest/adblock/ultimate.txt
    name: HaGeZi's Multi Ultimate
    id: 1758564499
  - enabled: true
    url: https://cdn.jsdelivr.net/gh/hagezi/dns-blocklists@latest/adblock/fake.txt
    name: HaGeZi's Fake
    id: 1758564501
  - enabled: true
    url: https://cdn.jsdelivr.net/gh/hagezi/dns-blocklists@latest/adblock/popupads.txt
    name: HaGeZi's Pop-Up Ads
    id: 1758564502
  - enabled: true
    url: https://cdn.jsdelivr.net/gh/hagezi/dns-blocklists@latest/adblock/tif.txt
    name: HaGeZi's Threat Intelligence Feeds DNS Blocklist
    id: 1758564513
  - enabled: true
    url: https://cdn.jsdelivr.net/gh/hagezi/dns-blocklists@latest/adblock/nrd7.txt
    name: HaGeZi's Newly Registered Domains
    id: 1758564517
  - enabled: true
    url: https://cdn.jsdelivr.net/gh/hagezi/dns-blocklists@latest/adblock/doh-vpn-proxy-bypass.txt
    name: HaGeZi's DoH/VPN/TOR/Proxy Bypass
    id: 1758564504
  - enabled: true
    url: https://cdn.jsdelivr.net/gh/hagezi/dns-blocklists@latest/adblock/hoster.txt
    name: HaGeZi's Badware Hoster blocking
    id: 1758564507
  - enabled: true
    url: https://cdn.jsdelivr.net/gh/hagezi/dns-blocklists@latest/adblock/urlshortener.txt
    name: HaGeZi's URL Shortener
    id: 1758564518
  - enabled: true
    url: https://cdn.jsdelivr.net/gh/hagezi/dns-blocklists@latest/adblock/spam-tlds.txt
    name: HaGeZi's Most Abused TLDs
    id: 1758564519
  - enabled: true
    url: https://cdn.jsdelivr.net/gh/hagezi/dns-blocklists@latest/adblock/spam-tlds-adblock-allow.txt
    name: HaGeZi's Allowlist Referral
    id: 1758564556
  - enabled: true
    url: https://cdn.jsdelivr.net/gh/hagezi/dns-blocklists@latest/adblock/anti.piracy.txt
    name: HaGeZi's Anti Piracy
    id: 1758564520
  - enabled: true
    url: https://cdn.jsdelivr.net/gh/hagezi/dns-blocklists@latest/adblock/gambling.txt
    name: HaGeZi's Gambling Full
    id: 1758564508
  - enabled: true
    url: https://cdn.jsdelivr.net/gh/hagezi/dns-blocklists@latest/adblock/nsfw.txt
    name: HaGeZi's NSFW
    id: 1758564527
  - enabled: true
    url: https://cdn.jsdelivr.net/gh/hagezi/dns-blocklists@latest/adblock/native.amazon.txt
    name: HaGeZi's Amazon Tracker
    id: 1758564509
  - enabled: true
    url: https://cdn.jsdelivr.net/gh/hagezi/dns-blocklists@latest/adblock/native.winoffice.txt
    name: HaGeZi's Windows/Office Tracker
    id: 1758564510
  - enabled: true
    url: https://cdn.jsdelivr.net/gh/hagezi/dns-blocklists@latest/adblock/native.samsung.txt
    name: HaGeZi's Samsung Tracker
    id: 1758564511
  - enabled: true
    url: https://cdn.jsdelivr.net/gh/hagezi/dns-blocklists@latest/adblock/native.tiktok.extended.txt
    name: HaGeZi's TikTok Tracker Fingerprinting Agressive
    id: 1758564512
  - enabled: true
    url: https://cdn.jsdelivr.net/gh/hagezi/dns-blocklists@latest/adblock/native.apple.txt
    name: HaGeZi's Apple Tracker
    id: 1758564514
  - enabled: true
    url: https://big.oisd.nl/
    name: oisd big
    id: 1758564515
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_1.txt
    name: AdGuard DNS filter
    id: 1
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_10.txt
    name: Scam Blocklist by DurableNapkin
    id: 1758564521
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_11.txt
    name: Malicious URL Blocklist (URLHaus)
    id: 1758564522
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_12.txt
    name: Dandelion Sprout's Anti-Malware List
    id: 1758564523
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_18.txt
    name: Phishing Army
    id: 1758564529
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_23.txt
    name: WindowsSpyBlocker - Hosts spy rules
    id: 1758564534
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_24.txt
    name: 1Hosts (Lite) ads, trackers, malware
    id: 1758564535
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_30.txt
    name: Phishing URL Blocklist (PhishTank and OpenPhish)
    id: 1758564541
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_31.txt
    name: Stalkerware Indicators List
    id: 1758564542
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_42.txt
    name: ShadowWhisperer's Malware List
    id: 1758564553
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_6.txt
    name: Dandelion Sprout's Game Console Adblock List
    id: 1758564569
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_7.txt
    name: Perflyst and Dandelion Sprout's Smart-TV Blocklist
    id: 1758564570
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_8.txt
    name: NoCoin Filter List
    id: 1758564571
  - enabled: true
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_9.txt
    name: The Big List of Hacked Malware Web Sites
    id: 1758564572
  - enabled: false
    url: https://adguardteam.github.io/HostlistsRegistry/assets/filter_2.txt
    name: AdAway Default Blocklist (Open-source ad blocker for Android using the hosts file)
    id: 2
  - enabled: true
    url: https://raw.githubusercontent.com/AdguardTeam/FiltersRegistry/master/filters/filter_11_Mobile/filter.txt
    name: Adguard Mobile Advertising
    id: 1758564573
  - enabled: true
    url: https://raw.githubusercontent.com/AdguardTeam/FiltersRegistry/master/filters/filter_14_Annoyances/filter.txt
    name: Adguard Cookies
    id: 1758564574
  - enabled: true
    url: https://raw.githubusercontent.com/AdguardTeam/FiltersRegistry/master/filters/filter_16_French/filter.txt
    name: Adguard French Advertising
    id: 1758564575
  - enabled: true
    url: https://raw.githubusercontent.com/AdguardTeam/FiltersRegistry/master/filters/filter_17_TrackParam/filter.txt
    name: Adguard General Tracking
    id: 1758564576
  - enabled: true
    url: https://raw.githubusercontent.com/AdguardTeam/FiltersRegistry/master/filters/filter_2_Base/filter.txt
    name: Adguard Advertising
    id: 1758564577
  - enabled: true
    url: https://raw.githubusercontent.com/AdguardTeam/FiltersRegistry/master/filters/filter_3_Spyware/filter.txt
    name: Adguard JS, CSS, HTML Extensions
    id: 1758564578
  - enabled: true
    url: https://raw.githubusercontent.com/AdguardTeam/FiltersRegistry/master/filters/filter_4_Social/filter.txt
    name: Adguard Social Trackers
    id: 1758564579
  - enabled: true
    url: https://sebsauvage.net/hosts/raw
    name: sebsauvage
    id: 1758564516
  - enabled: true
    url: https://blocklistproject.github.io/Lists/abuse.txt
    name: The Block List Project Abuse
    id: 1758564596
  - enabled: true
    url: https://blocklistproject.github.io/Lists/ads.txt
    name: The Block List Project Ads
    id: 1758564597
  - enabled: true
    url: https://blocklistproject.github.io/Lists/drugs.txt
    name: The Block List Project Drugs
    id: 1758564598
  - enabled: true
    url: https://blocklistproject.github.io/Lists/fraud.txt
    name: The Block List Project Fraud
    id: 1758564599
  - enabled: true
    url: https://blocklistproject.github.io/Lists/gambling.txt
    name: The Block List Project Gambling
    id: 1758564600
  - enabled: true
    url: https://blocklistproject.github.io/Lists/malware.txt
    name: The Block List Project Malware
    id: 1758564601
  - enabled: true
    url: https://blocklistproject.github.io/Lists/ransomware.txt
    name: The Block List Project Ransomware
    id: 1758564602
  - enabled: true
    url: https://blocklistproject.github.io/Lists/scam.txt
    name: The Block List Project Scam
    id: 1758564603
  - enabled: true
    url: https://blocklistproject.github.io/Lists/tracking.txt
    name: The Block List Project Tracking
    id: 1758564604
  - enabled: true
    url: https://blocklistproject.github.io/Lists/tiktok.txt
    name: The Block List Project TikTok
    id: 1758564605
  - enabled: true
    url: https://raw.githubusercontent.com/matomo-org/referrer-spam-blacklist/master/spammers.txt
    name: Matomo Referrer Spam
    id: 1758564587
  - enabled: true
    url: https://raw.githubusercontent.com/easylist/easylist/refs/heads/master/easylist_cookie/easylist_cookie_general_block.txt
    name: Easylist Cookies
    id: 1758564608
  - enabled: true
    url: https://easylist-downloads.adblockplus.org/antiadblockfilters.txt
    name: Adblock Warning Removal List
    id: 1758564609
  - enabled: true
    url: https://dl.red.flag.domains/red.flag.domains.txt
    name: Red Flag Domains
    id: 1758564610
  - enabled: true
    url: https://v.firebog.net/hosts/Prigent-Crypto.txt
    name: Prigent-Crypto
    id: 1758564614
  - enabled: true
    url: https://v.firebog.net/hosts/Prigent-Malware.txt
    name: Prigent-Malware
    id: 1758564615
  - enabled: true
    url: https://raw.githubusercontent.com/hkamran80/blocklists/refs/heads/main/smart-tv.txt
    name: Smart TV Blocklist
    id: 1758564617
  - enabled: true
    url: https://raw.githubusercontent.com/uBlockOrigin/uAssets/master/filters/filters.txt
    name: uBlock filters
    id: 1759494018
  - enabled: true
    url: https://raw.githubusercontent.com/uBlockOrigin/uAssets/master/filters/privacy.txt
    name: uBlock filters – Privacy
    id: 1759494019
  - enabled: true
    url: https://raw.githubusercontent.com/uBlockOrigin/uAssets/master/filters/resource-abuse.txt
    name: uBlock filters – Resource abuse
    id: 1759494020
  - enabled: true
    url: https://raw.githubusercontent.com/uBlockOrigin/uAssets/master/filters/quick-fixes.txt
    name: uBlock filters – Quick fixes
    id: 1759494021
  - enabled: true
    url: https://adguardteam.github.io/AdGuardSDNSFilter/Filters/filter.txt
    name: AdGuard DNS filter
    id: 1759494022
  - enabled: true
    url: https://secure.fanboy.co.nz/fanboy-annoyance.txt
    name: Fanboy's Annoyance List
    id: 1759494023
  - enabled: true
    url: https://secure.fanboy.co.nz/fanboy-cookiemonster.txt
    name: Easylist Cookie List
    id: 1759494024
  - enabled: true
    url: https://raw.githubusercontent.com/crazy-max/WindowsSpyBlocker/master/data/hosts/spy.txt
    name: WindowsSpyBlocker - Hosts spy rules
    id: 1759494025
  - enabled: true
    url: https://raw.githubusercontent.com/DandelionSprout/adfilt/master/Alternate%20versions%20Anti-Malware%20List/AntiMalwareAdGuardHome.txt
    name: Dandelion Sprout's Anti-Malware List
    id: 1759494026
  - enabled: true
    url: https://raw.githubusercontent.com/DandelionSprout/adfilt/master/GameConsoleAdblockList.txt
    name: Game Console Adblock List
    id: 1759494027
  - enabled: true
    url: https://raw.githubusercontent.com/durablenapkin/scamblocklist/master/adguard.txt
    name: Scam Blocklist by DurableNapkin
    id: 1759494028
  - enabled: true
    url: https://raw.githubusercontent.com/mitchellkrogza/The-Big-List-of-Hacked-Malware-Web-Sites/master/hosts
    name: The Big List of Hacked Malware Web Sites
    id: 1759494029
  - enabled: true
    url: https://raw.githubusercontent.com/nextdns/native-tracking-domains/main/domains/alexa
    name: NextDNS Alexa
    id: 1759494030
  - enabled: true
    url: https://raw.githubusercontent.com/nextdns/native-tracking-domains/main/domains/samsung
    name: NextDNS Samsung
    id: 1759494031
  - enabled: true
    url: https://raw.githubusercontent.com/nextdns/native-tracking-domains/main/domains/windows
    name: NextDNS Windows
    id: 1759494032
  - enabled: true
    url: https://raw.githubusercontent.com/Perflyst/PiHoleBlocklist/master/SmartTV-AGH.txt
    name: Smart-TV Blocklist for AdGuard Home (by Dandelion Sprout)
    id: 1759494033
  - enabled: true
    url: https://raw.githubusercontent.com/PolishFiltersTeam/KADhosts/master/KADhosts.txt
    name: KADhosts
    id: 1759494034
  - enabled: true
    url: https://raw.githubusercontent.com/StevenBlack/hosts/master/alternates/fakenews/hosts
    name: StevenBlack/hosts with the fakenews extension
    id: 1759494035
  - enabled: true
    url: https://easylist.to/easylist/easylist.txt
    name: EasyList
    id: 1759494036
  - enabled: true
    url: https://easylist.to/easylist/easyprivacy.txt
    name: EasyPrivacy
    id: 1759494037
  - enabled: true
    url: https://filters.adtidy.org/extension/ublock/filters/14.txt
    name: AdGuard Annoyances filter
    id: 1759494038
  - enabled: true
    url: https://gitlab.com/quidsup/notrack-blocklists/raw/master/notrack-malware.txt
    name: NoTrack Malware Blocklist
    id: 1759494039
  - enabled: true
    url: https://malware-filter.gitlab.io/malware-filter/urlhaus-filter-agh.txt
    name: Malicious URL Blocklist (AdGuard Home)
    id: 1759494040
  - enabled: true
    url: https://www.github.developerdan.com/hosts/lists/dating-services-extended.txt
    name: Lightswitch05's dating-services-extended
    id: 1759494041
```

```yaml
# Les règles de filtrage personnalisées sont fournis par  Zogg :
# - https://zogg.fr
# - https://git.zogg.fr/kraoc/agh

# Replace "user_rules" entry with these :p

user_rules:
  - '#'
  - '# BLOCK'
  - '#'
  - '# Windows 11'
  - '||arc.msn.com^$important'
  - '||ris.api.iris.microsoft.com^$important'
  - '||api.msn.com^$important'
  - '||assets.msn.com^$important'
  - '||c.msn.com^$important'
  - '||ntp.msn.com^$important'
  - '||srtb.msn.com^$important'
  - '||www.msn.com^$important'
  - '||fd.api.iris.microsoft.com^$important'
  - '||staticview.msn.com^$important'
  - '#'
  - '# ALLOW'
  - '#'
  - '# DNS'
  - '@@||dns0.eu^$important'
  - '@@||dns.sb^$important'
  - '@@||dot.sb^$important'
  - '@@||fdn.fr^$important'
  - '@@||cloudflare-dns.com^$important'
  - '@@||one.one.one.one^$important'
  - '@@||dns.quad9.net^$important'
  - '@@||mullvad.net^$important'
  - '@@||dnscheck.tools^$important'
  - '# Zogg'
  - '@@||zogg.fr^$important'
  - '# Blabla Linux'
  - '@@||blablalinux.be^important
  - '# Homelab'
  - '@@||ntp.org^$important'
  - '@@||get.geojs.io^$important'
  - '# Drivers'
  - '@@||events.gfe.nvidia.com^$important'
  - '@@||keychron.fr^$important'
  - '@@||keychron.com^$important'
  - '# !!! Windows 11'
  - '# Certificats'
  - '@@||ctldl.windowsupdate.com^$important'
  - '@@||ocsp.digicert.com^$important'
  - '# !!! Windows 11'
  - '# Devices metadata'
  - '@@||dmd.metaservices.microsoft.com^$important'
  - '# Microsoft account'
  - '@@||login.live.com^$important'
  - '# OneDrive'
  - '@@||g.live.com^$important'
  - '@@||onedrive.live.com^$important'
  - '@@||oneclient.sfx.ms^$important'
  - '@@||logincdn.msauth.net^$important'
  - '# Microsoft Defender'
  - '@@||wdcp.microsoft.com^$important'
  - '@@||smartscreen-prod.microsoft.com^$important'
  - '@@||checkappexec.microsoft.com^$important'
  - '@@||smartscreen.microsoft.com^$important'
  - '@@||ping-edge.smartscreen.microsoft.com^$important'
  - '@@||data-edge.smartscreen.microsoft.com^$important'
  - '@@||nav-edge.smartscreen.microsoft.com^$important'
  - '# Microsoft Store'
  - '@@||img-prod-cms-rt-microsoft-com.akamaized.net^$important'
  - '@@||img-s-msn-com.akamaized.net^$important'
  - '@@||storeedgefd.dsx.mp.microsoft.com^$important'
  - '@@||livetileedge.dsx.mp.microsoft.com^$important'
  - '@@||wns.windows.com^$important'
  - '@@||storecatalogrevocation.storequality.microsoft.com^$important'
  - '@@||displaycatalog.mp.microsoft.com^$important'
  - '# Microsoft Edge'
  - '@@||msedge.api.cdp.microsoft.com^$important'
  - '# Network connection indicator'
  - '@@||www.msftconnecttest.com^$important'
  - '@@||ipv6.msftconnecttest.com^$important'
  - '# Settings'
  - '@@||settings-win.data.microsoft.com^$important'
  - '@@||settings.data.microsoft.com^$important'
  - '@@||pipe.aria.microsoft.com^$important'
  - '# Teams'
  - '@@||config.teams.microsoft.com^$important'
  - '@@||teams.live.com^$important'
  - '@@||teams.events.data.microsoft.com^$important'
  - '@@||statics.teams.cdn.live.net^$important'
  - '@@||teams.events.data.microsoft.com^$important'
  - '# Windows Updates'
  - '@@||definitionupdates.microsoft.com^$important'
  - '@@||prod.do.dsp.mp.microsoft.com^$important'
  - '@@||dl.delivery.mp.microsoft.com^$important'
  - '@@||windowsupdate.com^$important'
  - '@@||delivery.mp.microsoft.com^$important'
  - '@@||update.microsoft.com^$important'
  - '@@||adl.windows.com^$important'
  - '@@||tsfe.trafficshaping.dsp.mp.microsoft.com^$important'
  - '@@||api.cdp.microsoft.com^$important'
  - '# Fonts diffusion'
  - '@@||fs.microsoft.com^$important'
  - '# Licenses'
  - '@@||licensing.mp.microsoft.com^$important'
  - '# Location'
  - '@@||inference.location.live.net^$important'
  - '# Mobile'
  - '@@||mobile.events.data.microsoft.com^$important'
  - '@@||eu-mobile.events.data.microsoft.com^$important'
  - '# !!! Sites'
  - '# Divers'
  - '@@||l.allocine.fr^$important'
  - '@@||t.notif-colissimo-laposte.info^$important'
  - '@@||image.jeuxvideo.com^$important'
  - '@@||static.jvc.gg^$important'
  - '@@||l.leparisien.fr^$important'
  - '@@||cdn.shopify.com^$important'
  - '@@||l.20minutes.fr^$important'
  - '@@||cloudimperiumgames.com^$important'
  - '@@||robertsspaceindustries.com^$important'
  - '@@||erkul.games^$important'
  - '@@||finder.cstone.space^$important'
  - '@@||cdprojekt.red^$important'
  - '# !!! Socials'
  - '# Tiktok'
  - '@@||www.tiktok.com^$important'
  - '@@||m.tiktok.com^$important'
  - '@@||tiktokw.eu^$important'
  - '@@||tiktokv.com^$important'
  - '@@||tiktokcdn.com^$important'
  - '@@||tiktokcdn-eu.com^$important'
  - '# Twitter (X)'
  - '@@||t.co^$important'
  - '# YouTube'
  - '@@||googlevideo.com^$important'
  - '# Discord'
  - '@@||cdn.discordapp.com^$important'
```
