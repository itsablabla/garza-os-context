# Known Issues & Workarounds

**Updated: 2026-04-12 — Devin infrastructure audit**

---

## Critical Issues

### H1-CONTAINERS-DOWN: 9 services returning 502 on H1
```
Severity: CRITICAL
Affected: research, agentzero, search, akaunting, invio, superglue, maxkb, maxkb1, mcpfactory-nomad
Server: Hostinger H1 (72.62.86.63)

Cause: Docker containers likely stopped. Traefik is running (SSL certs valid,
healthy services still responding) but backend containers are down.

Fix:
1. SSH into H1: ssh root@72.62.86.63
2. Check containers: docker ps -a
3. Restart stopped containers: docker compose up -d (in respective dirs)
4. Or restart all: docker restart $(docker ps -aq --filter status=exited)

Blocker: VPS_ROOT_PASSWORD is currently rejected — need updated password.
```

### SSH-AUTH-FAIL: SSH password rejected on H1 and H2
```
Severity: CRITICAL
Affected: H1 (72.62.86.63), H2 (187.77.25.131)

Cause: VPS_ROOT_PASSWORD Devin secret no longer authenticates.
Possible reasons: password rotated, SSH password auth disabled, or
root account locked.

Fix:
1. Log into Hostinger panel → VPS → Console (or reset password)
2. Update Devin secret VPS_ROOT_PASSWORD
3. Or add SSH key via Hostinger API/panel
```

### PROTONMAIL-MCP-DOWN: ProtonMail MCP returning 502
```
Severity: HIGH
URL: https://protonmail.garzaos.cloud/mcp

Fix: Same as H1-CONTAINERS-DOWN — container needs restart on H1.
```

---

## Medium Issues

### RAILWAY-TOKEN-EXPIRED: Railway API auth failing
```
Severity: MEDIUM
Secret: RAILWAY_API_TOKEN

Symptoms: GraphQL API returns "Not Authorized"
Fix: Regenerate at https://railway.com/account/tokens and update Devin secret.
```

### COMPOSIO-MCP-TIMEOUT: Composio MCP tools server times out
```
Severity: MEDIUM
URL: https://connect.composio.dev/mcp

Symptoms: MCP tool listing times out after 300s.
Workaround: Use direct HTTP POST to the Composio API with proper headers,
or use native Devin git tools (git_create_pr, git_view_pr, etc.) for GitHub operations.
```

---

## Low Issues

### AUTO2-5-ORPHAN-DNS: Stale DNS records for auto2-5.garzaos.cloud
```
Severity: LOW
DNS records exist pointing to H1 but no Traefik routes configured (404).

Fix options:
1. Deploy Sim Studio instances to auto2-5
2. Or remove stale DNS records via Hostinger API
```

### H2-UNKNOWN: H2 server purpose unclear
```
Severity: LOW
IP: 187.77.25.131
Ports open: 22, 80, 443

Server is running (Hostinger KVM 2) but no known services are mapped to it.
Need SSH access to inspect what's running.
```

### ID-UNKNOWN: Infomaniak "Garza" server purpose unclear
```
Severity: LOW
IP: 83.228.193.101
Ports open: 22, 80, 443, 8443

Serving nginx default page on HTTP. Need Infomaniak dashboard or SSH to inspect.
```

### INFOMANIAK-API: Cloud instance API endpoints returning errors
```
Severity: LOW
Symptoms: /2/cloud/{id}/instances returns "method_not_found"

Workaround: Use Infomaniak web dashboard for cloud management.
API may require different endpoint format for Jelastic/cloud instances.
```

---

## Resolved (from previous version)

| Issue | Status | Notes |
|-------|--------|-------|
| BEEPER-500 | N/A | Beeper MCP moved to different architecture |
| CF-SHELL-500 | N/A | CF MCP retired, using Hostinger VPS now |
| FLY-DEPLOY-HANG | N/A | Fly.io services migrated to Hostinger/Railway |

---

## Quick Fixes Cheat Sheet

| Problem | Quick Fix |
|---------|-----------|
| Service 502 on H1 | SSH in, restart Docker container |
| SSH auth fails | Reset password via Hostinger panel |
| Railway API 401 | Regenerate token at railway.com |
| Composio MCP timeout | Use Devin native git tools instead |
| Orphan DNS records | Delete via Hostinger API |
| SSL cert about to expire | Traefik auto-renews; check if container is running |

---

## Escalation

If workarounds don't help:
1. Check Hostinger VPS console via web panel
2. Use Hostinger API to restart VPS: POST /api/vps/v1/virtual-machines/{id}/restart
3. Check Fabric AI notes for recent changes
4. Alert Jaden via message
