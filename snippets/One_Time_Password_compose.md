# One Time Password compose

Fichier "docker-compose.yml" à utiliser pour déployer OTS (One Time Password) via Docker.

• docker
• compose
• yml
• ots
• time
• password

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
services:
  app:
    build:
      context: https://github.com/Luzifer/ots.git#v1.17.2
    restart: always
    container_name: ots
    environment:
      CUSTOMIZE: '/etc/ots/customize.yaml'
      REDIS_URL: redis://redis:6379/0
      SECRET_EXPIRY: "604800"
      STORAGE_TYPE: redis
    depends_on:
      - redis
    ports:
      - 3000:3000
    volumes:
      - ./config-ots:/etc/ots
  redis:
    image: redis:alpine@sha256:a0fc425a443bb583175bc380874f1cbfa55ccfeb0b1d690adf50b1e6d4e79513
    restart: always
    container_name: redis
    volumes:
      - ./data-redis:/data
```

```yaml
# Override the app-icon, present a path to the image to use, if unset
# or empty the default FontAwesome icon will be displayed. Recommended
# is a height of 30px.
appIcon: ''

# Alternative icon for the dark-themed frontend to the `appIcon`. If
# not set it will fall back to the `appIcon` (and therefore to the
# FontAwesome icon when that one is also unset).
appIconDark: ''

# Override the app-title, if unset or empty the default app-title
# "OTS - One Time Secrets" will be used
appTitle: ''

# Disable display of the app-title (for example if you included the
# title within the appIcon)
disableAppTitle: false

# Disable the dropdown and the API functionality to override the
# secret expiry
disableExpiryOverride: false

# Disable the footer linking back to the project. If you disable it
# please consider a donation to support the project.
#
# If you are enabling this option and are a business using OTS please
# consider supporting the development of OTS. Have a look at the main
# repository page for options.
disablePoweredBy: false

# Disable the button to display and the generation of the QR-Code
# for the secret URL
disableQRSupport: false

# Disable the switcher for dark / light theme in the top right corner
# for example if your custom theme does not support two themes.
disableThemeSwitcher: false

# Override the choices to be displayed in the expiry dropdown. Values
# are given in seconds and the order of the values controls the order
# in the dropdown.
expiryChoices: [300, 1800, 3600, 14400, 43200, 86400, 259200, 604800]

# Custom path to override embedded resources. You can override any
# file present in the `frontend` directory (which is baked into the
# binary during compile-time). You also can add new files (for
# example the appIcon given above). Those files are available at the
# root of the application (i.e. an app.png would be served at
# https://ots.example.com/app.png).
overlayFSPath: ''

# Switch to formal translations for languages having those defined.
# Languages not having a formal version will still display the normal
# translations in the respective language.
useFormalLanguage: false

# Define which file types are selectable by the user when uploading
# files to attach. This fuels the `accept` attribute of the file
# select and requires the same format.
#
# Pay attention this is NOT suited as a security measure as this is
# purely a frontend implementation and can easily be circumvented. The
# client tool does NOT check those file types and the server is not
# able to check them as it never sees the real content of the file.
#
# https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/file#accept
acceptedFileTypes: ''

# Disable the file attachment functionality alltogether. Keep in mind
# this also is just a frontend configuration. The server cannot see
# whether it got a 11kb secret or a 1kb secret and a 10kb file
# attached as they are encrypted into one single blob.
disableFileAttachment: false

# Define how big all attachments might be in bytes. Leave it set to
# zero to use the internal limit of 64 MiB (which is there to ensure
# the encrypted object does not cause the frontend to break).
#
# This size is checked by the frontend, not on the server side! To
# limit the size of data someone can send you limit the maximum body
# size of the request in the proxy in front of OTS.
maxAttachmentSizeTotal: 0

# Set a maximum size for secrets in total. Secrets bigger than this
# will be rejected by the server and not stored.
#
# When setting this keep in mind the attachments (which maximum size
# you've configured above) are base64 encoded and will grow approx
# to 4/3 of their former size. Afterwards they are combined with the
# secret and again base64 encoded (meaning another size increase).
# Given an attachment size of 64MiB and the formula `64*(4/3)*(4/3)`
# this would mean the secret could grow to ~120MiB in total size.
#
# The internal default uses the 64MiB max attachment size set by the
# frontend, increased by 1MiB and multiplied for base64 encoding:
maxSecretSize: 121168782 # 65 * 1024 * 1024 * (16 / 9) = 115.6MiB

# Allow access to /metrics endpoint for Prometheus metrics collection.
# Specify subnets to allow access from: Default is an empty list which
# denies all access to the metrics endpoint. When specifying a single
# IP you need to specify it in subnet notation: `127.0.0.1/32`
#
# To allow access from all sources (and then limit the endpoint in the
# proxy / ingress for security reason) use this list:
#metricsAllowedSubnets: ['0.0.0.0/0', '::/0']
metricsAllowedSubnets: [] # Default: Deny all
```
