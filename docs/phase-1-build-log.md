# CATENA Phase 1 — Build Log

## Infrastructure Setup

### Server Details
- Provider: Hetzner
- OS: Ubuntu 22.04 LTS
- Plan: CX22 (2 vCPU, 4GB RAM)
- Server name: catena-core
- IP: 204.168.176.138

### What's Installed
- [x] Ubuntu 22.04 secured
- [x] Non-root user (catena) created
- [x] UFW firewall configured (ports 22, 80, 443, 5678)
- [x] Docker installed
- [x] n8n running on port 5678

### Issues Encountered & Fixed
1. n8n crash-loop on first launch
   - Cause: Missing N8N_SECURE_COOKIE=false flag
   - Fix: Added environment variable to docker run command

2. EACCES permission denied on /home/node/.n8n/config
   - Cause: n8n-data folder owned by root, n8n runs as UID 1000
   - Fix: sudo chown -R 1000:1000 ~/catena/n8n-data

### Lessons Learned
- Always check docker logs first when a container is crash-looping
- Hetzner cloud firewall is separate from UFW — check both
- n8n newer versions require N8N_SECURE_COOKIE=false for HTTP

## Next Steps
- [ ] Connect Outlook to n8n
- [ ] Connect Google Tasks
- [ ] Set up Telegram bot as command channel
- [ ] Build Morning Brief workflow