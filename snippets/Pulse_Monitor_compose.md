# Pulse Monitor compose

Fichier "docker-compose.yml" à utiliser pour déployer Pulse Monitor via Docker.

• docker
• compose
• yml
• monitor
• pulse
• alert

```yaml
# Modifications apportées par Blabla Linux : https://link.blablalinux.be
services:
  pulse:
    image: rcourtman/pulse:latest
    container_name: pulse
    ports:
      - 7655:7655
    volumes:
      - ./data:/data
    environment:
      - TZ=Europe/Brussels  # Set your timezone
      - PULSE_PUBLIC_URL=https://pulse.blablalinux.be  # Full URL to access Pulse (e.g., http://192.168.1.100:7655)
      #- PUID=1000  # Optional: Set user ID (uncomment and adjust as needed)
      #- PGID=1000  # Optional: Set group ID (uncomment and adjust as needed)

      # NOTE: Env vars override UI settings. Remove env var to allow UI configuration.

      # Network discovery (usually not needed - auto-scans common networks)
      - DISCOVERY_SUBNET=192.168.2.0/24  # Only for non-standard networks

      # Ports
      #- PORT=7655                         # Backend port (default: 7655)
      #- FRONTEND_PORT=7655                # Frontend port (default: 7655)

      # Security (all optional - runs open by default)
      #- PULSE_AUTH_USER=admin             # Username for web UI login
      #- PULSE_AUTH_PASS=your-password     # Plain text or bcrypt hash (auto-hashed if plain)
      #- API_TOKEN=your-token              # Plain text or SHA3-256 hash (auto-hashed if plain)
      #- ALLOW_UNPROTECTED_EXPORT=false    # Allow export without auth (default: false)

      # Security: Plain text credentials are automatically hashed
      # You can provide either:
      # 1. Plain text (auto-hashed): PULSE_AUTH_PASS=mypassword
      # 2. Pre-hashed (advanced): PULSE_AUTH_PASS='$$2a$$12$$...'
      #    Note: Escape $ as $$ in docker-compose.yml for pre-hashed values

      # Performance
      #- CONNECTION_TIMEOUT=10             # Connection timeout in seconds (default: 10)

      # CORS & logging
      #- ALLOWED_ORIGINS=https://app.example.com  # CORS origins (default: none, same-origin only)
      #- LOG_LEVEL=info                    # Log level: debug/info/warn/error (default: info)
    restart: always
    healthcheck:
      test:
        - CMD
        - wget
        - --no-verbose
        - --tries=1
        - --spider
        - http://localhost:7655
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 30s
volumes:
  pulse_data:
    driver: local

```
