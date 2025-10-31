# DrawIO compose

Fichier "docker-compose.yml" à utiliser pour déployer DrawIO via Docker.

• docker
• yml
• draw
• compose

```yaml
# Modifications apportées par Blabla Linux : https: //link.blablalinux.be
services:
  drawio:
    image: jgraph/drawio:latest
    container_name: drawio
    restart: always
    ports:
      - 8080:8080
      - 8443:8443
    #environment:
      #PUBLIC_DNS: domain
      #ORGANISATION_UNIT: unit
      #ORGANISATION: org
      #CITY: city
      #STATE: state
      #COUNTRY_CODE: country
    healthcheck:
      test: ["CMD-SHELL", "curl -f http://localhost:8080 || exit 1"]
      interval: 1m30s
      timeout: 10s
      retries: 5
      start_period: 10s
```
