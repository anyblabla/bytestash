# Fail2ban compose

Fichier "docker-compose.yml" à utiliser pour déployer Fail2ban via Docker. Exemple prévu pour analyser les logs de NPM (Nginx Proxy Manager). Je propose quatre exemples de fichiers pour le répertoire "jail.d".

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
      - /data/compose/1/data/logs:/var/log:ro # chemin logs NPM à personnaliser
    environment:
      - TZ=Europe/Brussels # time zone à personnaliser
      - F2B_LOG_TARGET=STDOUT
      - F2B_LOG_LEVEL=INFO
      - F2B_DB_PURGE_AGE=28d # purge logs fail2ban à personnaliser
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

#failregex = ^\s*(?:\[\] )?\S+ \S+ (?:[4-5]0\d) - \S+ \S+ \S+ "[^"]*" \[Client <ADDR>\] (?:\[\w+ [^\]]*\] )*\[Sent-to <F-CONTAINER>[^\]]*</F-CONTAINER>\] "<F-USERAGENT>[^"]*</F-USERAGENT>"

failregex = ^\s*(?:\[\] )?\S+ \S+ (?:(?P<nf>404)|[4-5]0(?!4)\d|504) - \S+ \S+ \S+ "(?(nf)/(?:[^/]*/)*[^\.]*(?!\.(?:png|txt|jpg|ico|js|css|ttf|woff|woff2)[^\."]*)|[^"]*)" \[Client <ADDR>\] (?:\[\w+ [^\]]*\] )*\[Sent-to <F-CONTAINER>[^\]]*</F-CONTAINER>\] "<F-USERAGENT>[^"]*</F-USERAGENT>"
```

```plaintext
# à placer dans le répertoire "jail.d"

[DEFAULT]

# "bantime.increment" allows to use database for searching of previously banned ip's to increase a
# default ban time using special formula, default it is banTime * 1, 2, 4, 8, 16, 32...
bantime.increment = true


# "bantime.rndtime" is the max number of seconds using for mixing with random time
# to prevent "clever" botnets calculate exact time IP can be unbanned again:
bantime.rndtime = 1200


# "bantime.maxtime" is the max number of seconds using the ban time can reach (don't grows further)
#bantime.maxtime =


# "bantime.factor" is a coefficient to calculate exponent growing of the formula or common multiplier,
# default value of factor is 1 and with default value of formula, the ban time
# grows by 1, 2, 4, 8, 16 ...
bantime.factor = 1


# "bantime.formula" used by default to calculate next value of ban time, default value bellow,
# the same ban time growing will be reached by multipliers 1, 2, 4, 8, 16, 32...
bantime.formula = ban.Time * (1<<(ban.Count if ban.Count<20 else 20)) * banFactor

# more aggressive example of formula has the same values only for factor "2.0 / 2.885385" :
#bantime.formula = ban.Time * math.exp(float(ban.Count+1)*banFactor)/math.exp(1*banFactor)


# "bantime.multipliers" used to calculate next value of ban time instead of formula, coresponding
# previously ban count and given "bantime.factor" (for multipliers default is 1);
# following example grows ban time by 1, 2, 4, 8, 16 ... and if last ban count greater as multipliers count,
# always used last multiplier (64 in example), for factor '1' and original ban time 600 - 10.6 hours
#bantime.multipliers = 1 2 4 8 16 32 64

# following example can be used for small initial ban time (bantime=60) - it grows more aggressive at begin,
# for bantime=60 the multipliers are minutes and equal: 1 min, 5 min, 30 min, 1 hour, 5 hour, 12 hour, 1 day, 2 day
#bantime.multipliers = 1 5 30 60 300 720 1440 2880


[npm]
enabled = true
ignoreself = true
ignoreip =  127.0.0.1/8 ::1 # A good idea is to mention your LAN IP address range (example: 192.168.2.0/24) as well as your WAN IP.
chain = DOCKER-USER
logpath = /var/log/proxy-host-*_access.log
maxretry = 5
findtime = 1h
bantime  = 10h
action = docker-action
```

```plaintext
# à placer dans le répertoire "jail.d"

[DEFAULT]

# "bantime.increment" allows to use database for searching of previously banned ip's to increase a
# default ban time using special formula, default it is banTime * 1, 2, 4, 8, 16, 32...
bantime.increment = true


# "bantime.rndtime" is the max number of seconds using for mixing with random time
# to prevent "clever" botnets calculate exact time IP can be unbanned again:
bantime.rndtime = 1200


# "bantime.maxtime" is the max number of seconds using the ban time can reach (don't grows further)
#bantime.maxtime =


# "bantime.factor" is a coefficient to calculate exponent growing of the formula or common multiplier,
# default value of factor is 1 and with default value of formula, the ban time
# grows by 1, 2, 4, 8, 16 ...
#bantime.factor = 1


# "bantime.formula" used by default to calculate next value of ban time, default value bellow,
# the same ban time growing will be reached by multipliers 1, 2, 4, 8, 16, 32...
#bantime.formula = ban.Time * (1<<(ban.Count if ban.Count<20 else 20)) * banFactor

# more aggressive example of formula has the same values only for factor "2.0 / 2.885385" :
#bantime.formula = ban.Time * math.exp(float(ban.Count+1)*banFactor)/math.exp(1*banFactor)


# "bantime.multipliers" used to calculate next value of ban time instead of formula, coresponding
# previously ban count and given "bantime.factor" (for multipliers default is 1);
# following example grows ban time by 1, 2, 4, 8, 16 ... and if last ban count greater as multipliers count,
# always used last multiplier (64 in example), for factor '1' and original ban time 600 - 10.6 hours
#bantime.multipliers = 1 2 4 8 16 32 64

# following example can be used for small initial ban time (bantime=60) - it grows more aggressive at begin,
# for bantime=60 the multipliers are minutes and equal: 1 min, 5 min, 30 min, 1 hour, 5 hour, 12 hour, 1 day, 2 day
bantime.multipliers = 1 5 30 60 300 720 1440 2880


[npm]
enabled = true
ignoreself = true
ignoreip =  127.0.0.1/8 ::1 # A good idea is to mention your LAN IP address range (example: 192.168.2.0/24) as well as your WAN IP.
chain = DOCKER-USER
logpath = /var/log/proxy-host-*_access.log
maxretry = 5
findtime = 1h
bantime  = 1h
action = docker-action
```

```plaintext
# à placer dans le répertoire "jail.d"

[DEFAULT]

# "bantime.increment" allows to use database for searching of previously banned ip's to increase a
# default ban time using special formula, default it is banTime * 1, 2, 4, 8, 16, 32...
bantime.increment = true


# "bantime.rndtime" is the max number of seconds using for mixing with random time
# to prevent "clever" botnets calculate exact time IP can be unbanned again:
bantime.rndtime = 1200


# "bantime.maxtime" is the max number of seconds using the ban time can reach (don't grows further)
#bantime.maxtime =


# "bantime.factor" is a coefficient to calculate exponent growing of the formula or common multiplier,
# default value of factor is 1 and with default value of formula, the ban time
# grows by 1, 2, 4, 8, 16 ...
#bantime.factor = 1


# "bantime.formula" used by default to calculate next value of ban time, default value bellow,
# the same ban time growing will be reached by multipliers 1, 2, 4, 8, 16, 32...
#bantime.formula = ban.Time * (1<<(ban.Count if ban.Count<20 else 20)) * banFactor

# more aggressive example of formula has the same values only for factor "2.0 / 2.885385" :
#bantime.formula = ban.Time * math.exp(float(ban.Count+1)*banFactor)/math.exp(1*banFactor)


# "bantime.multipliers" used to calculate next value of ban time instead of formula, coresponding
# previously ban count and given "bantime.factor" (for multipliers default is 1);
# following example grows ban time by 1, 2, 4, 8, 16 ... and if last ban count greater as multipliers count,
# always used last multiplier (64 in example), for factor '1' and original ban time 600 - 10.6 hours
bantime.multipliers = 1 2 4 8 16 32 64

# following example can be used for small initial ban time (bantime=60) - it grows more aggressive at begin,
# for bantime=60 the multipliers are minutes and equal: 1 min, 5 min, 30 min, 1 hour, 5 hour, 12 hour, 1 day, 2 day
#bantime.multipliers = 1 5 30 60 300 720 1440 2880


[npm]
enabled = true
ignoreself = true
ignoreip =  127.0.0.1/8 ::1 # A good idea is to mention your LAN IP address range (example: 192.168.2.0/24) as well as your WAN IP.
chain = DOCKER-USER
logpath = /var/log/proxy-host-*_access.log
maxretry = 5
findtime = 1h
bantime  = 10h
action = docker-action
```

```plaintext
# à placer dans le répertoire "jail.d"

[DEFAULT]

# "bantime.increment" allows to use database for searching of previously banned ip's to increase a
# default ban time using special formula, default it is banTime * 1, 2, 4, 8, 16, 32...
#bantime.increment = true


# "bantime.rndtime" is the max number of seconds using for mixing with random time
# to prevent "clever" botnets calculate exact time IP can be unbanned again:
bantime.rndtime = 1200


# "bantime.maxtime" is the max number of seconds using the ban time can reach (don't grows further)
#bantime.maxtime =


# "bantime.factor" is a coefficient to calculate exponent growing of the formula or common multiplier,
# default value of factor is 1 and with default value of formula, the ban time
# grows by 1, 2, 4, 8, 16 ...
#bantime.factor = 1


# "bantime.formula" used by default to calculate next value of ban time, default value bellow,
# the same ban time growing will be reached by multipliers 1, 2, 4, 8, 16, 32...
#bantime.formula = ban.Time * (1<<(ban.Count if ban.Count<20 else 20)) * banFactor

# more aggressive example of formula has the same values only for factor "2.0 / 2.885385" :
#bantime.formula = ban.Time * math.exp(float(ban.Count+1)*banFactor)/math.exp(1*banFactor)


# "bantime.multipliers" used to calculate next value of ban time instead of formula, coresponding
# previously ban count and given "bantime.factor" (for multipliers default is 1);
# following example grows ban time by 1, 2, 4, 8, 16 ... and if last ban count greater as multipliers count,
# always used last multiplier (64 in example), for factor '1' and original ban time 600 - 10.6 hours
#bantime.multipliers = 1 2 4 8 16 32 64

# following example can be used for small initial ban time (bantime=60) - it grows more aggressive at begin,
# for bantime=60 the multipliers are minutes and equal: 1 min, 5 min, 30 min, 1 hour, 5 hour, 12 hour, 1 day, 2 day
#bantime.multipliers = 1 5 30 60 300 720 1440 2880


[npm]
enabled = true
ignoreself = true
ignoreip =  127.0.0.1/8 ::1 # A good idea is to mention your LAN IP address range (example: 192.168.2.0/24) as well as your WAN IP.
chain = DOCKER-USER
logpath = /var/log/proxy-host-*_access.log
maxretry = 5
findtime = 1h
bantime  = 10h
action = docker-action
```
