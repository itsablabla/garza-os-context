# N8N Autonomous Agent v3

**Deployed:** 2025-12-26
**Workflow ID:** `4C5ONaeNq6hErgdo`
**Webhook:** `https://garzasync.app.n8n.cloud/webhook/garza-agent-v3`
**Schedule:** Every 15 minutes

## Purpose

Self-monitoring N8N agent that:
1. Detects failed workflow executions
2. Analyzes issues using Claude
3. Takes autonomous action (fix, activate, notify)
4. Reports results

## Architecture (19 Nodes)

```
┌─────────────────────────────────────────────────────────────────────────┐
│                           TRIGGERS                                       │
│  ┌─────────────────┐    ┌─────────────────┐                            │
│  │ Every 15 Min    │    │ Manual Webhook  │                            │
│  └────────┬────────┘    └────────┬────────┘                            │
│           │                      │                                      │
│           └──────────┬───────────┘                                      │
│                      ▼                                                  │
├─────────────────────────────────────────────────────────────────────────┤
│                        DATA GATHERING (Parallel)                        │
│  ┌─────────────────────┐         ┌─────────────────────┐              │
│  │ Get Failed Execs    │         │ Get All Workflows    │              │
│  │ (status=error,10)   │         │                      │              │
│  └──────────┬──────────┘         └──────────┬───────────┘              │
│             │                               │                           │
│             └───────────┬───────────────────┘                          │
│                         ▼                                               │
│              ┌─────────────────┐                                       │
│              │  Merge Data     │  ← CRITICAL: Combines parallel inputs │
│              └────────┬────────┘                                       │
├───────────────────────┼─────────────────────────────────────────────────┤
│                       ▼                                                 │
│                CONTEXT BUILDING                                         │
│  ┌─────────────────────────────────────────────────────────────────┐  │
│  │ Build Context                                                    │  │
│  │ - Filters out self + self-healing workflow                       │  │
│  │ - Maps workflow IDs to names                                     │  │
│  │ - Builds structured context object                               │  │
│  └────────────────────────┬────────────────────────────────────────┘  │
│                           ▼                                            │
│  ┌─────────────────────────────────────────────────────────────────┐  │
│  │ Prepare Claude Request                                           │  │
│  │ - Builds complete API request body in JS                        │  │
│  │ - Avoids inline expression parsing issues                        │  │
│  └────────────────────────┬────────────────────────────────────────┘  │
├───────────────────────────┼─────────────────────────────────────────────┤
│                           ▼                                            │
│                    CLAUDE REASONING                                    │
│  ┌─────────────────────────────────────────────────────────────────┐  │
│  │ Claude Tool Use (claude-sonnet-4-20250514)                         │  │
│  │ - System: "FIX issues if possible. Only notify for critical."   │  │
│  │ - Tools: fix_workflow, activate_workflow, send_notification,    │  │
│  │          no_action_needed                                        │  │
│  └────────────────────────┬────────────────────────────────────────┘  │
│                           ▼                                            │
│  ┌─────────────────┐     ┌─────────────────┐                          │
│  │ Parse Tool Calls│────►│ Has Tool Calls? │                          │
│  └─────────────────┘     └────────┬────────┘                          │
│                                   │                                    │
│                    ┌──────────────┴──────────────┐                    │
│                    ▼                              ▼                    │
│              Has Tools                       No Tools                  │
├────────────────────┼──────────────────────────────┼─────────────────────┤
│                    ▼                              ▼                    │
│             TOOL EXECUTION                  No Action Needed           │
│  ┌─────────────────────┐                  ┌─────────────────┐        │
│  │ Split Tool Calls    │                  │ Log: Healthy    │        │
│  └──────────┬──────────┘                  └────────┬────────┘        │
│             ▼                                      │                  │
│  ┌─────────────────────────────────────┐          │                  │
│  │ Route Tool Call (SWITCH)            │          │                  │
│  │ ├── fix_workflow → Get JSON → Apply │          │                  │
│  │ ├── activate_workflow → Toggle      │          │                  │
│  │ ├── send_notification → Pushcut     │          │                  │
│  │ └── no_action → Log                 │          │                  │
│  └──────────────────────┬──────────────┘          │                  │
│                         │                          │                  │
├─────────────────────────┼──────────────────────────┼───────────────────┤
│                         └────────────┬─────────────┘                  │
│                                      ▼                                │
│                              FINALIZATION                              │
│  ┌─────────────────────────────────────────────────────────────────┐  │
│  │ Finalize Run → Webhook Response                                  │  │
│  │ Returns: {runComplete, timestamp, actionsExecuted, summary}     │  │
│  └─────────────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────────────┘
```

## Tools Available to Claude

| Tool | Purpose | Implementation |
|------|---------|----------------|
| `fix_workflow` | Update workflow JSON | Currently logs only (manual review) |
| `activate_workflow` | Toggle workflow active state | PATCH /api/v1/workflows/{id} |
| `send_notification` | Pushcut notification | Critical issues only |
| `no_action_needed` | Confirm healthy | Logs healthy state |

## Safety Rails

1. **Self-exclusion:** Excludes own workflow ID and self-healing workflow
2. **No purchases:** System prompt explicitly forbids financial transactions
3. **Critical only:** Notifications restricted to critical issues
4. **Error handler:** Falls back to self-healing workflow `ScSmqCrvcDOkGVjM`
5. **Fix logging:** Apply Fix node logs for review rather than auto-applying

## Credentials

See Craft doc 7061 for credentials:
- N8N API Key
- Claude API Key  
- Pushcut Webhook ID

## Testing

```bash
# Manual trigger
curl -X POST "https://garzasync.app.n8n.cloud/webhook/garza-agent-v3"

# Check latest execution
curl "https://garzasync.app.n8n.cloud/api/v1/executions?workflowId=4C5ONaeNq6hErgdo&limit=1" \
  -H "X-N8N-API-KEY: <key>"
```

## Related Workflows

| Workflow | ID | Purpose |
|----------|----|---------|
| Self-Healing | `ScSmqCrvcDOkGVjM` | Error handler, D1 learning |
| Agent v3 | `4C5ONaeNq6hErgdo` | This workflow |

## Future Improvements

- [ ] Implement actual JSON modification in Apply Fix node
- [ ] Add success rate tracking to D1
- [ ] Multi-step fix attempts with validation
- [ ] Slack/Discord integration for non-critical updates
