# Open WebUI compose

Fichier "docker-compose.yml" à utiliser pour déployer Open WebUI via Docker.

• docker
• compose
• yml
• openwebui

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
services:
  ollama:
    volumes:
      - ./ollama:/root/.ollama
    container_name: ollama
    pull_policy: always
    tty: true
    restart: always
    image: ollama/ollama:latest
    environment:
      - OLLAMA_HOST=0.0.0.0:11434
    ports:
      - 11434:11434
    networks:
      - ollama-docker
    # Activer le support GPU NVIDIA OU AMD (l'un ou l'autre)
    # GPU support NVIDIA
    #deploy:
      #resources:
        #reservations:
          #devices: 
            #- driver: nvidia
              #count: 1
              #capabilities: [gpu]
    # GPU support AMD
    #devices:
      #- /dev/kfd:/dev/kfd
      #- /dev/dri:/dev/dri
    #image: ollama/ollama:rocm # Désactiver alors l'image "ollama/ollama:latest" ci-dessus
    #environment:
      #- 'HSA_OVERRIDE_GFX_VERSION=11.0.0'
  open-webui:
    image: ghcr.io/open-webui/open-webui:v0.6.16 # Ou l'image ghcr.io/open-webui/open-webui:cuda si le support NVIDIA est activé
    container_name: open-webui
    volumes:
      - ./open-webui:/app/backend/data
    depends_on:
      - ollama
    ports:
      - 3000:8080
    environment:
      - OLLAMA_BASE_URL=http://ollama:11434
      - OLLAMA_API_BASE_URL=http://ollama:11434/api
      - WEBUI_SECRET_KEY=CBWxhkhDpTrFpY
    extra_hosts:
      - host.docker.internal:host-gateway
    restart: always
    networks:
      - ollama-docker
volumes:
  ollama: {}
  open-webui: {}
networks:
  ollama-docker:
    driver: bridge
```
