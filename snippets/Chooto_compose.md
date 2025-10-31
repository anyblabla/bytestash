# Chooto compose

Fichier "docker-compose.yml" à utiliser pour déployer Chhoto via Docker.

• docker
• yml
• chhoto
• url
• compose

```yaml
# Modifications apportées par Blabla Linux : https: //link.blablalinux.be
services:
  chhoto-url:
    image: sintan1729/chhoto-url:latest
    restart: always
    container_name: chhoto-url
    ports:
      - 4567:4567
    environment:
      # Change if you want to mount the database somewhere else.
      # In this case, you can get rid of the db volume below
      # and instead do a mount manually by specifying the location.
      # Make sure that you create an empty file with the correct name 
      # before starting the container if you do make any changes.
      # (In fact, I'd suggest that you do that so that you can keep
      # a copy of your database.)
      # - db_url=/db/urls.sqlite

      # Change it in case you want to set the website name
      # displayed in front of the shorturls, defaults to
      # the hostname you're accessing it from.
      - site_url=https://chhoto.blablalinux.be

      - password=5GkcSa$ZvX*!w5

      # Pass the redirect method, if needed. TEMPORARY and PERMANENT
      # are accepted values, defaults to PERMANENT.
      # - redirect_method=TEMPORARY

      # By default, the auto-generated pairs are adjective-name pairs.
      # If you want UIDs, please change slug_style to UID.
      # Supported values for slug_style are Pair and UID.
      # The length is 8 by default, and a minimum of 4 is allowed.
      # - slug_style=Pair
      # - slug_length=8

      # In case you want to provide public access to adding links (and not 
      # delete, or listing), change the following option to Enable.
      - public_mode=Enable

      # By default, the server sends no Cache-Control headers. You can supply a 
      # comma separated list of valid header as per RFC 7234 §5.2 to send those
      # headers instead.
      # - cache_control_header=no-cache, private
    volumes:
      - ./db:/db

volumes:
    db:

```
