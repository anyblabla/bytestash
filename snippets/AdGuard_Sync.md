# AdGuard Sync

Fichier "docker-compose.yml" à utiliser pour déployer AdGuard Sync via Docker.

• docker
• compose
• yml
• adguard
• dns
• sync

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
services:
  adguardhome-sync:
    image: ghcr.io/bakito/adguardhome-sync:latest
    container_name: adguardhome-sync
    command: run --config /config/adguardhome-sync.yaml
    environment:
      - TZ=Europe/Brussels
    volumes:
      - ./config/adguardhome-sync.yaml:/config/adguardhome-sync.yaml
    ports:
      - 8080:8080
    restart: always
```

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be

# cron expression to run in daemon mode. (default; "" = runs only once)
cron: "0 */2 * * *"

# runs the synchronisation on startup
runOnStart: true

# If enabled, the synchronisation task will not fail on single errors, but will log the errors and continue
continueOnError: false

origin:
  # url of the origin instance
  url: https://192.168.2.218:443
  #apiPath: define an api path if other than "/control"
  insecureSkipVerify: true # disable tls check
  username: blabla
  password: '7BGAPo*6o5^3Ki'
  #cookie: Origin-Cookie-Name=CCCOOOKKKIIIEEE
  #requestHeaders: # Additional request headers
    #AAA: bbb

# replicas instances
replicas:
  # url of the replica instance
  - url: https://192.168.2.219:443
    insecureSkipVerify: true # disable tls check
    username: blabla
    password: '7BGAPo*6o5^3Ki'
    #cookie: Replica1-Cookie-Name=CCCOOOKKKIIIEEE
  #- url: http://192.168.1.4
    #username: username
    #password: password
    #cookie: Replica2-Cookie-Name=CCCOOOKKKIIIEEE
    #autoSetup: true # if true, AdGuardHome is automatically initialized.
    #webURL: "https://some-other.url" # used in the web interface (default: <replica-url>
    #requestHeaders: # Additional request headers
      #AAA: bbb

# Configure the sync API server, disabled if api port is 0
api:
  # Port, default 8080
  port: 8080
  # if username and password are defined, basic auth is applied to the sync API
  #username: root
  #password: 'blablalinux'
  # enable api dark mode
  darkMode: true

  # enable metrics on path '/metrics' (api port must be != 0)
  #metrics:
    #enabled: true
    #scrapeInterval: 30s 
    #queryLogLimit: 10000

  # enable tls for the api server
  #tls:
  #   # the directory of the provided tls certs
    #certDir: /path/to/certs
  #   # the name of the cert file (default: tls.crt)
    #certName: foo.crt
  #   # the name of the key file (default: tls.key)
    #keyName: bar.key

# Configure sync features; by default all features are enabled.
features:
  generalSettings: true
  queryLogConfig: false
  statsConfig: false
  clientSettings: true
  services: true
  filters: true
  dhcp:
    serverConfig: false
    staticLeases: false
  dns:
    serverConfig: true
    accessLists: true
    rewrites: false

```
