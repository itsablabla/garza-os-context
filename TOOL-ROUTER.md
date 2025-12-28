# Tool Router - Decision Tree

**Use this to pick the right tool for any task.**

## Quick Reference

| Task Category | Primary Tool | Backup |
|---------------|--------------|--------|
| Send message | `Garza Home MCP:beeper_send_message` | `Beeper:send_message` |
| Search messages | `Garza Home MCP:beeper_search_messages` | `Beeper:search_messages` |
| Read document | `Craft:blocks_get` | - |
| Create document | `Craft:documents_create` + `blocks_add` | - |
| Search documents | `Craft:documents_search` | - |
| Tasks | `Craft:tasks_*` | - |
| SSH command | `Last Rock Dev:ssh_exec` | `CF MCP:ssh_exec` |
| GitHub | `Last Rock Dev:github_*` | - |
| Fly.io | `Last Rock Dev:fly_*` | - |
| n8n | `Last Rock Dev:n8n_*` | - |
| Security (Abode) | `Garza Home MCP:abode_*` | `Garza Hive MCP:abode_*` |
| Cameras (UniFi) | `Garza Home MCP:unifi_*` | `CF MCP:unifi_*` |
| Email (ProtonMail) | `Garza Home MCP:*_protonmail` | `CF MCP:*_protonmail` |
| Cloudflare Workers | `Cloudflare Developer Platform:*` | - |
| Gmail | `Zapier:gmail_*` | Native Gmail tools |
| Calendar | `list_gcal_events`, `fetch_gcal_event` | - |

---

## Detailed Decision Trees

### 📱 MESSAGING (Beeper)

```
Need to send a message?
├── Know the chatID? → Garza Home MCP:beeper_send_message
└── Need to find chatID first?
    ├── Search by name → Garza Home MCP:beeper_search_chats
    └── Search by content → Garza Home MCP:beeper_search_messages
    
Need to read messages?
├── Specific chat → Garza Home MCP:beeper_list_messages
└── Search across all → Garza Home MCP:beeper_search_messages

Tool fails with 500?
└── Retry once, then try Beeper standalone: Beeper:*
```

### 📝 KNOWLEDGE (Craft)

```
Read a document?
├── Know the ID → Craft:blocks_get (format=markdown for reading)
├── Know the date → Craft:blocks_get (date="today" or "2025-01-01")
└── Need to find it → Craft:documents_search (use regexps)

Create content?
├── New document → Craft:documents_create then Craft:blocks_add
├── Add to existing → Craft:blocks_add (need position)
└── Quick markdown → Craft:markdown_add

Manage tasks?
├── View tasks → Craft:tasks_get (scope: active/upcoming/inbox)
├── Add task → Craft:tasks_add
└── Complete task → Craft:tasks_update (state: done)

Collections (databases)?
├── List all → Craft:collections_list
├── Get items → Craft:collectionItems_get
└── Add item → Craft:collectionItems_add
```

### 🔧 INFRASTRUCTURE (Last Rock Dev)

```
SSH/Shell command?
├── Any host → Last Rock Dev:ssh_exec (host: garzahive/mac/n8n)
├── Need hosts list → Last Rock Dev:ssh_hosts
└── CF MCP:ssh_exec as backup (host: mac)

GitHub?
├── List repos → Last Rock Dev:github_list_repos
├── Get file → Last Rock Dev:github_get_file
├── Update file → Last Rock Dev:github_update_file
├── Create repo → Last Rock Dev:github_create_repo
└── Workflows → Last Rock Dev:github_trigger_workflow

Fly.io?
├── List apps → Last Rock Dev:fly_list_apps
├── Get status → Last Rock Dev:fly_get_app
├── View logs → Last Rock Dev:fly_logs
├── Restart → Last Rock Dev:fly_restart
└── Set secret → Last Rock Dev:fly_set_secret

n8n?
├── List workflows → Last Rock Dev:n8n_list_workflows
├── Get workflow → Last Rock Dev:n8n_get_workflow
├── Execute → Last Rock Dev:n8n_execute_workflow
├── Toggle on/off → Last Rock Dev:n8n_activate_workflow
└── Check runs → Last Rock Dev:n8n_list_executions
```

### 🏠 HOME (Garza Home MCP)

```
Security (Abode)?
├── Get alarm mode → Garza Home MCP:abode_get_mode
├── Set alarm → Garza Home MCP:abode_set_mode (standby/home/away)
├── List devices → Garza Home MCP:abode_list_devices
└── Lock/unlock → Garza Home MCP:abode_lock_device

Cameras (UniFi)?
├── List cameras → Garza Home MCP:unifi_list_cameras
├── Get snapshot → Garza Home MCP:unifi_get_snapshot
├── Get events → Garza Home MCP:unifi_get_events
├── List sensors → Garza Home MCP:unifi_list_sensors
└── Control lights → Garza Home MCP:unifi_set_light

Email (ProtonMail)?
├── Search inbox → Garza Home MCP:search_protonmail
├── Read email → Garza Home MCP:read_protonmail
└── Send email → Garza Home MCP:send_protonmail

Knowledge Graph (Graphiti)?
├── Search → Garza Home MCP:graphiti_search
├── Get facts → Garza Home MCP:graphiti_get_facts
└── Add episode → Garza Home MCP:graphiti_add_episode

Bible?
├── Verse of day → Garza Home MCP:bible_votd
├── Get passage → Garza Home MCP:bible_passage
└── Search → Garza Home MCP:bible_search
```

### ☁️ CLOUDFLARE

```
Workers?
├── List workers → Cloudflare Developer Platform:workers_list
├── Get worker → Cloudflare Developer Platform:workers_get_worker
└── Get code → Cloudflare Developer Platform:workers_get_worker_code

D1 Database?
├── List DBs → Cloudflare Developer Platform:d1_databases_list
├── Query → Cloudflare Developer Platform:d1_database_query
└── Create → Cloudflare Developer Platform:d1_database_create

KV?
├── List namespaces → Cloudflare Developer Platform:kv_namespaces_list
└── Create → Cloudflare Developer Platform:kv_namespace_create

R2?
├── List buckets → Cloudflare Developer Platform:r2_buckets_list
└── Create → Cloudflare Developer Platform:r2_bucket_create
```

### 🔐 SECRETS & STATE

```
Get a secret?
└── CF MCP:get_secret (name)

List secrets?
└── CF MCP:list_secrets (optional category filter)

Store state?
├── Set → CF MCP:set_state (key, value)
└── Get → CF MCP:get_state (key)

Audit logging?
├── Log action → CF MCP:log_action
└── Get actions → CF MCP:get_session_actions
```

---

## Common Patterns

### Send message to Jessica
```
1. Garza Home MCP:beeper_search_chats query="Jessica" type="single"
2. Get chatID from result
3. Garza Home MCP:beeper_send_message chatID=X text="message"
```

### Deploy to Fly.io
```
1. Last Rock Dev:ssh_exec host="mac" command="cd /path && fly deploy"
2. Last Rock Dev:fly_logs app="appname" to verify
```

### Create Craft doc with content
```
1. Craft:documents_create destination={destination: "unsorted"} documents=[{title: "Title"}]
2. Craft:markdown_add position={pageId: "returned-id", position: "end"} markdown="content"
```

### Check if service is healthy
```
1. Last Rock Dev:fly_get_app app="servicename"
2. Check status field
3. If unhealthy: Last Rock Dev:fly_logs app="servicename"
```
