# iptag.conf

Le script IP-Tag applique automatiquement des balises basées sur IP aux conteneurs LXC et aux machines virtuelles dans Proxmox VE. Cela vous permet d'identifier rapidement les machines par IP (ou IP partielle) directement dans l'interface utilisateur Web de Proxmox.

• tag
• ip
• lxc
• vm
• proxmox

```plaintext
# https://github.com/community-scripts/ProxmoxVE/discussions/5790
#
# General Settings
#
# Tag format options:
# - "full": full IP address (e.g., 192.168.0.100)
# - "last_octet": only the last octet (e.g., 100)
# - "last_two_octets": last two octets (e.g., 0.100)
TAG_FORMAT="full"
# Only tag private subnets
CIDR_LIST=(
  192.168.0.0/16
  #10.0.0.0/8
)
# Lower background frequency
LOOP_INTERVAL=300
FORCE_UPDATE_INTERVAL=7200
# Update Intervals
VM_STATUS_CHECK_INTERVAL=600
LXC_STATUS_CHECK_INTERVAL=300
FW_NET_INTERFACE_CHECK_INTERVAL=900
#
#
# Performance Tuning
#
# VM Performance
VM_IP_CACHE_TTL=300
MAX_PARALLEL_VM_CHECKS=2
# LXC Performance
LXC_IP_CACHE_TTL=300
LXC_STATUS_CACHE_TTL=300
LXC_BATCH_SIZE=3
MAX_PARALLEL_LXC_CHECKS=2
LXC_AGGRESSIVE_CACHING=true
LXC_SKIP_SLOW_METHODS=true
```
