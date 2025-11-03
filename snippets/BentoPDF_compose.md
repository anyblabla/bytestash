# BentoPDF compose

Fichier "docker-compose.yml" à utiliser pour déployer BentoPDF via Docker

• docker
• compose
• pdf
• bento
• tools

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
services:
  bentopdf:
    # simple mode - bentopdf/bentopdf-simple:latest
    # default mode - bentopdf/bentopdf:latest
    image: bentopdf/bentopdf-simple:latest
    container_name: bentopdf
    restart: always
    ports:
      - 8080:8080
```
