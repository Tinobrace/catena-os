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
## HTTPS Setup
- Subdomain created: n8n.valencloud.xyz → 204.168.176.138
- Nginx installed and configured as reverse proxy
- SSL certificate issued via Let's Encrypt / Certbot
- n8n now only accessible via HTTPS
- Port 5678 closed to public, routed internally via Nginx
- Permanent fix: chown -R 1000:1000 baked into start-n8n.sh

## Issues Fixed
3. 502 Bad Gateway after HTTPS setup
   - Cause: permissions wiped when container was recreated
   - Fix: chown -R 1000:1000 ~/catena/n8n-data before docker run
   - Prevention: added fix permanently to start-n8n.sh

## Outlook Connection
- Personal Microsoft account connected to n8n
- Azure App registered: CATENA-n8n
- App ID: a5f81782-082e-4d53-b2e1-3e3be3024e04
- Permissions: Mail.Read, Mail.Send, Calendars.Read, Tasks.ReadWrite
- Common OAuth URLs used for personal account
- Issue fixed: copied secret ID instead of secret Value
- [x] Outlook credential live and confirmed working in n8n

## Google Tasks Connection
- Google Cloud project created: CATENA
- APIs enabled: Google Tasks, Gmail, Google Calendar
- OAuth client registered: CATENA-n8n
- 4 Google accounts connected:
  - val.ukah01@gmail.com
  - tinobracex640@gmail.com
  - ucheukah01@gmail.com
  - valen.uchenna@gmail.com
- Fix applied: redirect_uri_mismatch — corrected URI in Google Cloud
- Fix applied: second account auth — recreated credentials cleanly
- [x] All 4 Google Tasks accounts live in n8n

## Morning Brief — LIVE
- First successful execution: 2026-03-20
- Claude AI generating intelligent summaries
- Delivering to Telegram daily at 7:45am
- Fixed: Always Output Data on all nodes
- Fixed: Node name references in HTTP Request body
- Fixed: Raw mode + expression toggle for JSON body
- CATENA identified real security risks on first run

## Gmail Integration
- Gmail connected to Morning Brief workflow
- Primary account: val.ukah01@gmail.com
- Brief now covers: Google Calendar + Google Tasks + Outlook + Gmail
- Decision: one primary Gmail for Morning Brief, others reserved for specific workflows
