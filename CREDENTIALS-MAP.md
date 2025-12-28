# Credentials Map

**Where to find credentials for GARZA OS systems.**

> ⚠️ This file contains LOCATIONS, not actual credentials.

---

## Primary Sources

### 1. Supabase Vault (Preferred)
```
Tool: CF MCP:get_secret
Tool: CF MCP:list_secrets

Categories:
- ai (OpenAI, Anthropic, etc.)
- communication (Beeper, ProtonMail)
- infrastructure (Fly, Cloudflare, GitHub)
- home (Abode, UniFi)
- business (Stripe, Chargebee)
```

### 2. Craft Document 7061
```
Tool: Craft:blocks_get id="7061" format="markdown"
Contains: Legacy credentials, some still active
Note: Being migrated to Supabase vault
```

---

## Quick Lookup

| System | Vault Key | Craft Location |
|--------|-----------|----------------|
| OpenAI | `openai_api_key` | 7061 |
| Anthropic | `anthropic_api_key` | 7061 |
| GitHub PAT | `github_pat` | 7061 |
| Fly.io | `fly_api_token` | 7061 |
| n8n | `n8n_api_key` | 7061 |
| Beeper | `beeper_*` | 7061 |
| ProtonMail | `protonmail_*` | 7061 |
| Abode | `abode_*` | 7061 |
| UniFi | `unifi_*` | 7061 |
| Cloudflare | `cloudflare_*` | 7061 |
| Supabase | `supabase_*` | 7061 |
| Scout APM | `scout_*` | 7061 |

---

## Important Craft Docs

| Doc ID | Contents |
|--------|----------|
| 7061 | API keys and passwords |
| 9239 | IP addresses |
| 14219 | GARZA OS Master Config |
| 14522 | Jada Soul Doc |
| 17348 | MCP Tool Audit |
| 20684 | Infrastructure Docs |
| 21668 | Prompts |

---

## MCP Server URLs & Keys

### Garza Home MCP
```
URL: https://garza-home-mcp.fly.dev/sse
Key: garza-home-v2-26f93afcebe2ea974cceeddbddeb4fdb
```

### Last Rock Dev
```
URL: https://lastrock-mcp.garzahive.com/sse
Key: lrlab-dev-v2-a7c9e3f18b2d4e6f
```

### CF MCP
```
URL: https://mcp-cf.garzahive.com/sse
Key: a505d6e718eec02e2ee95ec3fcd6c0623197b013d143b993dcfa4d5eb60eaf0c
```

### Garza Hive MCP
```
URL: https://mcp.garzahive.com/sse
Key: a63bed37c48272d6f2e4d87e59953348ffe65edf58919654527644fe84b9c85c
```

---

## SSH Hosts

| Alias | Description | Access Via |
|-------|-------------|------------|
| mac | Jaden's Mac | Last Rock Dev, CF MCP |
| garzahive | DigitalOcean VPS | Last Rock Dev, CF MCP |
| n8n | n8n server (146.190.157.249) | Last Rock Dev |

---

## Notes

- Rotate keys after any suspected exposure
- New credentials should go to Supabase vault
- Legacy Craft doc 7061 maintained for backup
- Never store credentials in GitHub repos
