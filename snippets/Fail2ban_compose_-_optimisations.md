# Fail2ban compose - optimisations

Fichier "docker-compose.yml" à utiliser pour déployer Fail2ban via Docker. Exemple prévu pour analyser les logs de NPM (Nginx Proxy Manager).

• yml
• docker
• compose
• fail2ban
• nginx
• npm

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
# Page de wiki disponible à cette adresse : https://wiki.blablalinux.be/fr/docker-compose-fail2ban
services:
  fail2ban:
    image: crazymax/fail2ban:latest
    container_name: fail2ban
    network_mode: host
    cap_add:
      - NET_ADMIN
      - NET_RAW
    volumes:
      - ./data:/data
      - /root/npm/data/logs:/var/log:ro
    environment:
      - TZ=Europe/Brussels
      - F2B_LOG_TARGET=STDOUT
      - F2B_LOG_LEVEL=INFO
      - F2B_DB_PURGE_AGE=28d
    restart: always

```

```plaintext
# à placer dans le répertoire "action.d"

[Definition]

actionstart = iptables -N f2b-npm-docker
              iptables -A f2b-npm-docker -j RETURN
              iptables -I FORWARD -p tcp -m multiport --dports 0:65535 -j f2b-npm-docker

actionstop = iptables -D FORWARD -p tcp -m multiport --dports 0:65535 -j f2b-npm-docker
             iptables -F f2b-npm-docker
             iptables -X f2b-npm-docker

actioncheck = iptables -n -L FORWARD | grep -q 'f2b-npm-docker[ \t]'

actionban = iptables -I f2b-npm-docker -s <ip> -j DROP

actionunban = iptables -D f2b-npm-docker -s <ip> -j DROP
```

```plaintext
# à placer dans le répertoire "filter.d"

[INCLUDES]

[Definition]

failregex = ^\s*(?:\[\] )?\S+ \S+ (?:(?P<nf>404)|(?P<auth_fail>400|401|403)|[4-5]0(?!0|1|3|4)\d|504) - \S+ \S+ \S+ "(?(nf)/(?:[^/]*/)*[^\.]*(?!\.(?:png|txt|jpg|ico|js|css|ttf|woff|woff2)[^\."]*)|[^"]*)" \[Client <ADDR>\] (?:\[\w+ [^\]]*\] )*\[Sent-to <F-CONTAINER>[^\]]*</F-CONTAINER>\] "<F-USERAGENT>[^"]*</F-USERAGENT>"
```

```markdown
## Structure Générale du Log Ciblé

L'expression regex est conçue pour correspondre à un format de log d'accès Nginx typique, souvent enrichi par NPM ou par des modules comme `log_format combined`.

  * **`^\s*(?:\[\] )?\S+ \S+`** : Débute la ligne. Ignore l'espace initial, capture des champs standards comme le temps et l'hôte (par exemple, la date et le nom du serveur) qui varient avant le code de statut.
  * **`\[Client <ADDR>\]`** : C'est le marqueur essentiel utilisé par Fail2Ban pour identifier l'adresse IP source, grâce au motif de capture `<ADDR>`.
  * **Reste de la ligne** : La regex ignore les informations restantes après l'adresse IP, comme les en-têtes (conteneur, user-agent) pour se concentrer sur le code d'erreur et l'adresse.

-----

## Les Codes d'Erreur HTTP Capturés (Le cœur de la règle)

La partie la plus complexe et la plus spécifique est celle qui capture les codes de statut HTTP :

```regexp
(?:(?P<nf>404)|(?P<auth_fail>400|401|403)|[4-5]0(?!0|1|3|4)\d|504)
```

Ce segment capture les tentatives échouées en les classant en plusieurs catégories (groupes nommés **`(?P<nom>...)`**) :

1.  **Tentatives d'Exploitation ou Accès Interdit (`400`, `401`, `403`) :**

      * **`(?P<auth_fail>400|401|403)`** : Ce groupe nommé capture spécifiquement les codes suivants :
          * **`400 Bad Request`** : Requête malformée, souvent utilisée lors de scans de vulnérabilités.
          * **`401 Unauthorized`** : Tentative d'accès à une ressource nécessitant une authentification (échec de connexion).
          * **`403 Forbidden`** : Tentative d'accès à une ressource dont l'accès est refusé (accès non autorisé).

2.  **Pages/Ressources Inexistantes (`404`) :**

      * **`(?P<nf>404)`** : Ce groupe nommé capture le code **`404 Not Found`**. Il est utilisé comme une *condition* pour la section suivante qui filtre les fichiers statiques.

3.  **Autres Erreurs Clients et Serveurs :**

      * **`[4-5]0(?!0|1|3|4)\d`** : Capture tous les codes d'erreur **4xx et 5xx** qui ne sont pas déjà capturés précédemment :
          * `4\d\d` : Tous les codes 4xx, *sauf* 400, 401, 403 et 404. Par exemple : `405 Method Not Allowed`, `408 Request Timeout`, etc.
          * `5\d\d` : Tous les codes d'erreur serveur 5xx (par exemple, `500 Internal Server Error`, `502 Bad Gateway`, `503 Service Unavailable`).
      * **`504`** : Le code **`504 Gateway Timeout`** est capturé explicitement à la fin.

-----

## La Spécificité du Filtrage 404

C'est la partie la plus avancée et l'une des **spécificités clés** de ce regex :

```regexp
"(?(nf)/(?:[^/]*/)*[^\.]*(?!\.(?:png|txt|jpg|ico|js|css|ttf|woff|woff2)[^\."]*)|[^"]*)"
```

Ce motif utilise une construction conditionnelle `(?(condition)then|else)` :

  * **Condition `(nf)`** : Si le code de statut capturé était un `404` (via le groupe nommé `nf` ci-dessus), alors la règle s'applique.
  * **Action si VRAI (404)** : `/(?:[^/]*/)*[^\.]*(?!\.(?:png|txt|jpg|ico|js|css|ttf|woff|woff2)[^\."]*`
      * Ceci est un filtre négatif. Il capture l'URL demandée **uniquement si elle NE se termine PAS** par des extensions de fichiers statiques courantes (`.png`, `.txt`, `.jpg`, `.ico`, `.js`, `.css`, `.ttf`, `.woff`, `.woff2`).
      * **But :** Empêcher le bannissement d'adresses IP dues à des erreurs légitimes (par exemple, un utilisateur qui n'arrive pas à charger une image ou une icône de site) tout en ciblant les scans qui demandent des fichiers d'administration ou des chemins inexistants (`/wp-admin`, `/phpmyadmin`, etc.).
  * **Action si FAUX (Non-404)** : `|[^"]*`
      * Si le code de statut n'est **pas** un 404 (par exemple 401, 500, etc.), la regex capture simplement n'importe quelle chaîne pour l'URL demandée (`[^"]*`) sans appliquer le filtre d'extensions.

En résumé, ce `failregex` est **très efficace** car il est à la fois **général** (couvrant la plupart des erreurs client/serveur) et **chirurgical** (filtrant intelligemment les 404 pour minimiser les faux positifs).
```

```plaintext
# à placer dans le répertoire "jail.d"

[DEFAULT]
bantime.increment = true
bantime.rndtime = 600
bantime.factor = 2
bantime.formula = ban.Time * (ban.Count if ban.Count<15 else 15) * banFactor
bantime.maxtime = 2592000

[npm]
enabled = true
ignoreself = false
ignoreip = 127.0.0.1/8 ::1 # ajouter plage d'adresses IP LAN en /24, IP hôte LXC, IP container NPM, et votre IP WAN(*) publique en /32 (ex.: ignoreip = 127.0.0.1/8 ::1 192.168.2.0/24 192.168.2.210 172.19.0.2 91.179.126.16/32) 
chain = DOCKER-USER
logpath = /var/log/proxy-host-*_access.log
maxretry = 3
findtime = 600
bantime = 1800
action = docker-action

# (*) Si vous avez une adresse IP WAN publique dunamique, voici une solution : https://wiki.blablalinux.be/fr/ip-dynamique-fail2ban
```

```markdown
## 1. Section `[DEFAULT]` : Stratégie de Bannissement Progressive

Cette section définit la stratégie globale de Fail2Ban pour punir les adresses IP persistantes. Le but est de bannir légèrement les erreurs isolées, mais d'appliquer des punitions de plus en plus sévères aux récidivistes.

| Paramètre | Valeur | Spécificité et Implication |
| :--- | :--- | :--- |
| **`bantime.increment`** | `true` | C'est la clé de la stratégie. Fail2Ban enregistre le nombre de bannissements précédents et **augmente la durée du bannissement** pour chaque nouvelle infraction. |
| **`bantime.rndtime`** | `600` (10 min) | Ajoute une durée aléatoire (jusqu'à 10 minutes) au temps de bannissement réel. Cela empêche les bots intelligents de calculer le moment exact du déblocage pour reprendre l'attaque. |
| **`bantime.factor`** | `2` | C'est le **multiplicateur** utilisé dans la formule. En réglant le facteur à `2`, la croissance du temps de bannissement est plus rapide et plus sévère. |
| **`bantime.formula`** | `ban.Time * (ban.Count if ban.Count<15 else 15) * banFactor` | Définit la manière dont le temps de bannissement augmente. Pour chaque récidive, la durée augmente par `ban.Count * 2` (grâce au facteur). L'exposant est plafonné à 15, garantissant que le temps de bannissement n'augmente pas à l'infini (un excellent garde-fou). |
| **`bantime.maxtime`** | `2592000` (30 jours) | Définit la **durée maximale** absolue d'un bannissement. Même avec la formule d'incrémentation, un attaquant ne pourra pas être banni pour plus de 30 jours, ce qui est une bonne pratique de gestion. |

---

## 2. Section `[npm]` : Jail pour Nginx Proxy Manager

Ceci est le jail spécifique qui surveille les logs de Nginx Proxy Manager (NPM).

| Paramètre | Valeur | Spécificité et Implication |
| :--- | :--- | :--- |
| **`enabled`** | `true` | Active le jail. |
| **`ignoreself`** | `false` | Désactive la tentative d'auto-détection du nom d'hôte de Fail2Ban (pour éviter le *warning* que nous avons corrigé). L'auto-protection est gérée manuellement via `ignoreip`. |
| **`ignoreip`** | (Liste d'IPs) | Liste des IPs et des plages **jamais bannies**. Elle est exhaustive et bien configurée pour votre environnement : IPs locales, IP de l'hôte LXC (`192.168.2.210`), IP interne du conteneur NPM (`172.19.0.2`), et votre IP publique (`91.179.126.16/32`). |
| **`chain`** | `DOCKER-USER` | **Crucial** pour Docker. Fail2Ban insère ses règles `iptables` dans la chaîne `DOCKER-USER`, s'assurant que l'IP est bloquée **avant** que les règles de Docker ne transfèrent le trafic vers vos conteneurs. C'est le niveau de protection optimal. |
| **`logpath`** | `/var/log/proxy-host-*_access.log` | Cible les fichiers de logs d'accès de Nginx Proxy Manager (le joker `*` permet de cibler tous les domaines gérés). |
| **`maxretry`** | `3` | **Stratégie agressive.** Une adresse IP n'a droit qu'à 3 tentatives échouées avant d'être bannie. |
| **`findtime`** | `600` (10 min) | La fenêtre de temps dans laquelle les 3 tentatives (`maxretry`) doivent se produire. 10 minutes est idéal pour cibler les scanners et les attaques rapides, tout en épargnant les utilisateurs légitimes qui font des erreurs espacées. |
| **`bantime`** | `1800` (30 min) | **Durée de bannissement initial.** C'est une durée relativement courte (30 min). L'intention est de ne pas punir trop sévèrement une simple erreur, tout en comptant sur la stratégie progressive de la section `[DEFAULT]` pour punir lourdement les récidivistes. |
| **`action`** | `docker-action` | Utilise votre action `iptables` personnalisée pour Docker, qui insère les règles dans la chaîne `FORWARD` pour bloquer le trafic conteneur. |

En résumé, ce jail privilégie une **réponse rapide et agressive** (`maxretry=3`, `findtime=10m`) pour les attaques, tout en utilisant une **stratégie de punition évolutive** (`bantime.increment=true`) qui réserve les bannissements longs et définitifs aux attaquants persistants. C'est une configuration de sécurité moderne et très efficace.
```
