# Known Issues & Workarounds

**Common problems and how to fix them.**

---

## MCP Server Issues

### BEEPER-500: Beeper tools return 500 errors
```
Symptoms: Any Beeper tool call returns HTTP 500
Frequency: Intermittent, ~20% of calls

Workarounds:
1. Retry the same call (usually works second time)
2. Use Beeper standalone MCP instead of Garza Home
3. If persistent, check Beeper service status
```

### SSH-TIMEOUT: SSH commands timeout
```
Symptoms: ssh_exec hangs for 30+ seconds then fails
Frequency: Common on long-running commands

Workarounds:
1. Use nohup: nohup command > /tmp/out.log 2>&1 &
2. Check results separately: cat /tmp/out.log
3. For deploys: Use GitHub Actions instead
```

### CF-SHELL-500: CF MCP shell_exec fails
```
Symptoms: shell_exec returns 500, ssh_exec works
Frequency: ~50% of shell_exec calls

Workaround:
- Use ssh_exec with host="mac" instead of shell_exec
- Example: CF MCP:ssh_exec host="mac" command="ls"
```

### CRAFT-TIMEOUT: Craft operations slow
```
Symptoms: Craft tool calls take 10+ seconds
Frequency: Occasional, usually on large docs

Workarounds:
1. Use maxDepth parameter to limit returned content
2. Split large operations into smaller chunks
3. Use format="markdown" for read-only operations
```

---

## Deployment Issues

### FLY-DEPLOY-HANG: Fly deploy hangs
```
Symptoms: fly deploy command never returns
Cause: SSH timeout before deploy completes

Workaround:
1. Use GitHub Actions workflow instead
2. Or: nohup fly deploy > /tmp/deploy.log 2>&1 &
3. Monitor: tail -f /tmp/deploy.log
```

### GITHUB-AUTH: GitHub push fails
```
Symptoms: "Authentication failed" on git push
Cause: PAT expired or SSH key issue

Fix:
1. Check if using SSH remote: git remote -v
2. If HTTPS with PAT, token may be expired
3. SSH should work: git@github.com:itsablabla/repo.git
```

### N8N-WEBHOOK-FAIL: n8n webhook doesn't trigger
```
Symptoms: Webhook POST returns 404 or no execution
Causes: Workflow inactive, wrong URL, auth required

Checks:
1. Verify workflow is active: n8n_list_workflows
2. Check webhook path matches exactly
3. Some webhooks need auth header
```

---

## Tool-Specific Issues

### GRAPHITI-EMPTY: Graphiti search returns nothing
```
Symptoms: graphiti_search returns empty results
Cause: No matching facts, or Graphiti service down

Checks:
1. Try broader search terms
2. Check if facts exist: graphiti_get_facts
3. May need to add episodes first
```

### UNIFI-AUTH: UniFi operations fail
```
Symptoms: UniFi tools return auth errors
Cause: Session expired, credentials changed

Fix:
1. Usually clears on retry
2. Check UniFi controller is accessible
3. Verify credentials in vault match controller
```

### PROTONMAIL-IMAP: Email operations slow/fail
```
Symptoms: ProtonMail tools timeout or fail
Cause: IMAP bridge connection issues

Workarounds:
1. Retry (usually works)
2. Use simpler search criteria
3. Limit results with limit parameter
```

---

## Environment Issues

### EGRESS-BLOCKED: Can't reach external URLs
```
Symptoms: web_fetch or curl fails on certain domains
Cause: Claude's container has egress restrictions

Allowed domains:
- api.anthropic.com
- github.com
- registry.npmjs.org
- pypi.org
- Various CDNs

Workaround:
- Use MCP tools instead of direct HTTP
- SSH through to a server that can reach the URL
```

### PATH-ISSUES: Command not found
```
Symptoms: flyctl, wrangler, etc. not found via SSH
Cause: PATH not set in SSH session

Fix:
export PATH="/Users/customer/.fly/bin:$PATH"
export PATH="/Users/customer/.npm-global/bin:$PATH"
```

---

## Quick Fixes Cheat Sheet

| Problem | Quick Fix |
|---------|-----------|
| Beeper 500 | Retry once |
| SSH timeout | Use nohup + background |
| shell_exec 500 | Use ssh_exec instead |
| GitHub auth | Check SSH key setup |
| Deploy hangs | Use GitHub Actions |
| Tool not found | Export PATH first |
| Craft slow | Add maxDepth limit |

---

## Escalation

If workarounds don't help:
1. Check service logs: `fly_logs` or SSH to check journalctl
2. Check if service is running: `fly_get_app`
3. Restart service: `fly_restart`
4. Check Craft doc 20684 for infrastructure details
5. Alert Jaden via Beeper if critical
