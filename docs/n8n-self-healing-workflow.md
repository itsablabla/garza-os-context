# N8N Self-Healing Workflow

**Deployed:** 2025-12-26
**Workflow ID:** `ScSmqCrvcDOkGVjM`
**Trigger:** Error workflow handler + Manual webhook
**Purpose:** Automatically detect, analyze, and fix failing N8N workflows

## Architecture

```
┌─────────────────────────────────────────────────────────────────────────┐
│                           TRIGGERS                                       │
│  ┌─────────────────────┐    ┌─────────────────┐                        │
│  │ On Error Trigger    │    │ Manual Webhook  │                        │
│  │ (workflow failures) │    │ /self-healing   │                        │
│  └──────────┬──────────┘    └────────┬────────┘                        │
│             └────────────┬───────────┘                                  │
│                          ▼                                              │
├─────────────────────────────────────────────────────────────────────────┤
│                    ERROR CONTEXT EXTRACTION                             │
│  ┌─────────────────────────────────────────────────────────────────┐  │
│  │ Get Error Details                                                │  │
│  │ - Workflow ID, name                                             │  │
│  │ - Error message, node that failed                               │  │
│  │ - Execution timestamp                                           │  │
│  └────────────────────────┬────────────────────────────────────────┘  │
├───────────────────────────┼─────────────────────────────────────────────┤
│                           ▼                                            │
│                   LEARNING LOOP (D1)                                   │
│  ┌─────────────────────────────────────────────────────────────────┐  │
│  │ Query Past Fixes                                                 │  │
│  │ - SELECT * FROM self_healing_history                            │  │
│  │ - WHERE workflow_id = ? OR error_pattern LIKE ?                 │  │
│  │ - Provides context of what worked before                        │  │
│  └────────────────────────┬────────────────────────────────────────┘  │
├───────────────────────────┼─────────────────────────────────────────────┤
│                           ▼                                            │
│                    CLAUDE ANALYSIS                                     │
│  ┌─────────────────────────────────────────────────────────────────┐  │
│  │ Analyze with Claude                                              │  │
│  │ - Error context + past fix history                              │  │
│  │ - Determines root cause                                         │  │
│  │ - Proposes fix with confidence score                            │  │
│  │ - Returns: fix_type, target_node, new_config, confidence        │  │
│  └────────────────────────┬────────────────────────────────────────┘  │
├───────────────────────────┼─────────────────────────────────────────────┤
│                           ▼                                            │
│                  CONFIDENCE ROUTING                                    │
│  ┌─────────────────────────────────────────────────────────────────┐  │
│  │ If confidence >= 70%:  AUTO-FIX                                 │  │
│  │   → Apply fix via N8N API                                       │  │
│  │   → Validate by running workflow                                │  │
│  │   → Rollback if validation fails                                │  │
│  │                                                                  │  │
│  │ If confidence < 70%:   NOTIFY ONLY                              │  │
│  │   → Send Pushcut notification                                   │  │
│  │   → Log for manual review                                       │  │
│  └────────────────────────┬────────────────────────────────────────┘  │
├───────────────────────────┼─────────────────────────────────────────────┤
│                           ▼                                            │
│                     LOGGING (D1)                                       │
│  ┌─────────────────────────────────────────────────────────────────┐  │
│  │ Record Fix Attempt                                               │  │
│  │ - workflow_id, error_pattern, fix_applied                       │  │
│  │ - success (boolean), confidence                                 │  │
│  │ - timestamp                                                      │  │
│  │ Future runs learn from this history                             │  │
│  └─────────────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────────────┘
```

## D1 Database

**Database:** `self_healing_history`
**Account:** garza-mcp (b09efd12-2be0-4d90-8a7a-dc4fcf16fc85)

### Schema

```sql
CREATE TABLE IF NOT EXISTS self_healing_history (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  workflow_id TEXT NOT NULL,
  workflow_name TEXT,
  error_pattern TEXT,
  fix_applied TEXT,
  success BOOLEAN DEFAULT FALSE,
  confidence REAL,
  created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
  validated_at DATETIME,
  rolled_back BOOLEAN DEFAULT FALSE
);

CREATE INDEX idx_workflow ON self_healing_history(workflow_id);
CREATE INDEX idx_error ON self_healing_history(error_pattern);
```

## Features

### Confidence Tiers
| Confidence | Action |
|------------|--------|
| 90%+ | Auto-fix immediately |
| 70-89% | Auto-fix with notification |
| 50-69% | Notify only, suggest fix |
| <50% | Notify only, manual review |

### Validation Loop
1. Apply fix via PATCH /api/v1/workflows/{id}
2. Trigger test execution
3. Wait for completion
4. Check execution status
5. If failed → rollback to original
6. Log result to D1

### Rollback Capability
- Original workflow JSON stored before fix
- Automatic restoration on validation failure
- Rollback flag set in D1 history

## Integration with Agent v3

The Self-Healing workflow (`ScSmqCrvcDOkGVjM`) is set as the error handler for Autonomous Agent v3 (`4C5ONaeNq6hErgdo`). If the agent fails, self-healing kicks in to analyze and repair.

## Testing

```bash
# Manual trigger
curl -X POST "https://garzasync.app.n8n.cloud/webhook/self-healing" \
  -H "Content-Type: application/json" \
  -d '{"workflowId": "test", "error": "Sample error"}'

# Check D1 history
curl "https://api.cloudflare.com/client/v4/accounts/{account}/d1/database/{db}/query" \
  -H "Authorization: Bearer {token}" \
  -d '{"sql": "SELECT * FROM self_healing_history ORDER BY created_at DESC LIMIT 10"}'
```

## Evolution History

1. **v1:** Basic error notification via Pushcut
2. **v2:** Added Claude analysis for root cause
3. **v3:** Added auto-fix application via N8N API
4. **v4:** Added D1 learning loop, validation, rollback
