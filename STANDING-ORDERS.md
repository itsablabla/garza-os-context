# Standing Orders

**Last updated:** 2025-12-28

These are persistent instructions for Claude. When Jaden says "keep going", "iterate", or "check standing orders", follow these.

---

## Active Orders

### 1. N8N Autonomous Agent v3
- **Status:** DEPLOYED ✅
- **Workflow ID:** 4C5ONaeNq6hErgdo
- **Webhook:** https://garzasync.app.n8n.cloud/webhook/garza-agent-v3
- **Schedule:** Every 15 minutes
- **Docs:** [docs/n8n-autonomous-agent-v3.md](docs/n8n-autonomous-agent-v3.md)
- **Action:** Monitor. If failing, fix it. Apply Fix node currently just logs - consider implementing actual JSON modification.

### 2. Self-Healing Workflow
- **Status:** DEPLOYED ✅
- **Workflow ID:** ScSmqCrvcDOkGVjM
- **Docs:** [docs/n8n-self-healing-workflow.md](docs/n8n-self-healing-workflow.md)
- **Action:** Monitor. This is the error handler for other workflows.

---

## Pending Work

_Add items here for Claude to work on autonomously_

1. (empty)

---

## Completed

- [x] Document all Dec 25-28 sessions to GitHub (2025-12-28)
- [x] Deploy Autonomous Agent v3 (2025-12-26)
- [x] Fix parallel branch merge issue
- [x] Fix Claude API JSON expression issue
- [x] Self-healing workflow with D1 learning
- [x] N8N Cloud migration (14 workflows)
- [x] Create STANDING-ORDERS.md system

---

## Documentation

| Doc | Description |
|-----|-------------|
| [n8n-autonomous-agent-v3.md](docs/n8n-autonomous-agent-v3.md) | Agent architecture, tools, safety rails |
| [n8n-self-healing-workflow.md](docs/n8n-self-healing-workflow.md) | Self-healing with D1 learning loop |
| [n8n-cloud-migration.md](docs/n8n-cloud-migration.md) | Migration from Docker to N8N Cloud |
| [SESSION-CHANGELOG.md](docs/SESSION-CHANGELOG.md) | Chronological session history |

---

## Rules

1. **Never make purchases** - No financial transactions without explicit confirmation
2. **Notify on critical failures** - Use Pushcut for anything urgent
3. **Update this file** - After completing work, move items to Completed
4. **Commit changes** - After any code/config changes, commit to GitHub

---

## How to Use

**Jaden says:** "Keep iterating" or "Check standing orders"
**Claude does:** 
1. Fetch this file
2. Work through Pending Work items
3. Update status
4. Commit changes

**Jaden says:** "Add to standing orders: [task]"
**Claude does:** 
1. Add to Pending Work section
2. Commit file
