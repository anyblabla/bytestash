# rwMarkable

Fichier "docker-compose.yml" à utiliser pour déployer rwMarkable via Docker. 

• docker
• compose
• yml
• rwmarkable
• notes

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
services:
  rwmarkable:
    image: ghcr.io/fccview/rwmarkable:latest
    container_name: rwmarkable
    #user: "1000:1000"
    ports:
      # Feel free to change the FIRST port, 3000 is very common
      # so I like to map it to something else (in this case 1122)
      - "1122:3000"
    volumes:
      # --- MOUNT DATA DIRECTORY
      # This is needed for persistent data storage on YOUR host machine rather than inside the docker volume.
      - ./data:/app/data:rw
      - ./config:/app/config:ro

      # --- MOUNT CACHE DIRECTORY (OPTIONAL)
      # This improves performance by persisting Next.js cache between restarts
      - ./cache:/app/.next/cache:rw
    restart: always
    environment:
      - NODE_ENV=production
      # Uncomment to enable HTTPS
      # - HTTPS=true

      # --- SSO WITH OIDC (OPTIONAL)
      # - SSO_MODE=oidc
      # - OIDC_ISSUER=<YOUR_SSO_ISSUER>
      # - OIDC_CLIENT_ID=<YOUR_SSO_CLIENT_ID>
      # - APP_URL=https://your-rwmarkable-domain.com # if not set should default to http://localhost:<port>

      # --- ADDITIONAL SSO OPTIONS (OPTIONAL)
      #- OIDC_CLIENT_SECRET=your_client_secret  # Enable confidential client mode with client authentication
      #- SSO_FALLBACK_LOCAL=true  # Allow both SSO and normal login
      #- OIDC_ADMIN_GROUPS=admins # Map provider groups to admin role
    # --- DEFAULT PLATFORM IS SET TO AMD64, UNCOMMENT TO USE ARM64.
    #platform: linux/arm64
```
