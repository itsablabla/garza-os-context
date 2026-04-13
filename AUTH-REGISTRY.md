# GARZA OS — Auth Registry

> **Comprehensive inventory of all authentication credentials, API keys, tokens, and service accounts across the GARZA OS ecosystem.**
>
> Last updated: 2026-04-13
>
> **This file contains credential REFERENCES and locations — not actual secret values.**
> Actual values live in Devin Secrets, Bitwarden Vault, or environment variables.

---

## Table of Contents

1. [Devin Stored Secrets](#1-devin-stored-secrets)
2. [LLM & AI Provider Keys](#2-llm--ai-provider-keys)
3. [VPS & Infrastructure](#3-vps--infrastructure)
4. [MCP Server Auth](#4-mcp-server-auth)
5. [Nextcloud](#5-nextcloud)
6. [Communication Services](#6-communication-services)
7. [Vault & Secret Management](#7-vault--secret-management)
8. [Nomad Internet / Business Services](#8-nomad-internet--business-services)
9. [App Logins (Web UIs)](#9-app-logins-web-uis)
10. [Railway Deployments](#10-railway-deployments)
11. [Repo-Specific Env Vars](#11-repo-specific-env-vars)
12. [Legacy / DigitalOcean Infrastructure](#12-legacy--digitalocean-infrastructure)
13. [Third-Party Integrations](#13-third-party-integrations)
14. [Credential Rotation Log](#14-credential-rotation-log)

---

## 1. Devin Stored Secrets

These are available as `${SECRET_NAME}` in all Devin sessions.

| Secret Name | Purpose | Service / URL |
|---|---|---|
| `ANTHROPIC_API_KEY` | Anthropic Claude API | api.anthropic.com |
| `BLINKO_API_TOKEN` | Blinko notes API JWT (legacy) | jadennotes.pikapod.net |
| `CHARGEBEE_API_KEY` | Chargebee billing API | nomad-internet.chargebee.com |
| `CHARGEBEE_SITE` | Chargebee site ID (`nomad-internet`) | — |
| `COMPOSIO_MCP_API_KEY` | Composio MCP consumer key | connect.composio.dev/mcp |
| `FABRIC_API_KEY` | Fabric AI notes/memory API | api.fabric.so |
| `FREESCOUT_ADMIN_PASSWORD` | FreeScout admin login | support.nomad-os.cloud |
| `FREESCOUT_MCP_API_KEY` | FreeScout custom MCP server | support.nomad-os.cloud |
| `HOSTINGER_API_TOKEN` | Hostinger DNS/VPS management | api.hostinger.com |
| `INFOMANIAK_API_TOKEN` | Infomaniak DNS/hosting | api.infomaniak.com |
| `NEXTCLOUD_APP_PASSWORD` | Nextcloud admin app password | next.garzaos.online |
| `NEXTCLOUD_USERNAME` | Nextcloud admin username | next.garzaos.online |
| `PROTONMAIL_MCP_TOKEN` | ProtonMail MCP Bearer token | protonmail.garzaos.cloud/mcp |
| `RAILWAY_API_TOKEN` | Railway deployment API | railway.com |
| `VAULT_ADMIN_TOKEN` | Bitwarden Vault admin panel | vault.garzaos.cloud/admin |
| `VAULT_MCP_BEARER_TOKEN` | Bitwarden Vault MCP Bearer | vault.garzaos.cloud/mcp |
| `VAULT_MCP_URL` | Vault MCP endpoint URL | vault.garzaos.cloud |
| `VERIZON_BASIC_AUTH` | ThingSpace OAuth basic auth | thingspace.verizon.com |
| `VERIZON_PASSWORD` | ThingSpace session password | thingspace.verizon.com |
| `VERIZON_SECRET_KEY` | ThingSpace client secret | thingspace.verizon.com |
| `VERIZON_TOKEN_URL` | ThingSpace OAuth token URL | thingspace.verizon.com |
| `VERIZON_USERNAME` | ThingSpace session username | thingspace.verizon.com |
| `VPS_IP` | Hostinger VPS IP (72.62.86.63) | — |
| `VPS_ROOT_PASSWORD` | SSH root password for VPS | 72.62.86.63 |

---

## 2. LLM & AI Provider Keys

| Provider | Devin Secret | Used By |
|---|---|---|
| Anthropic (Claude) | `ANTHROPIC_API_KEY` | Superglue, various agents |
| OpenRouter | `OPENROUTER_API_KEY` | Jada Agent backend (Hermes) |
| Google Gemini | `GOOGLE_GEMINI_API_KEY` | Jada Agent (Gemini 2.5 Flash) |
| Kimi K2.5 | `KIMI_API_KEY` | Experimental |
| OpenAI | Bitwarden Vault | Garza Home Engine, Superglue |
| Fabric AI | `FABRIC_API_KEY` | Garza MCP, primary memory store |

### Fabric AI Details
- **API Base**: `https://api.fabric.so`
- **Project Folder ID**: `89cd201a-0be0-47f2-a25e-bdc1f85c1ef8` ("GARZA OS — Agent Handoff")
- **Endpoints**: `/v2/notepads`, `/v2/memories`, `/v2/search`
- **Auth**: `X-Api-Key` header

---

## 3. VPS & Infrastructure

### Hostinger VPS (Primary — 72.62.86.63)
| Item | Secret / Value |
|---|---|
| IP Address | `$VPS_IP` (72.62.86.63) |
| SSH Root Password | `$VPS_ROOT_PASSWORD` |
| VPS ID | 1561515 |
| Hostinger API | `$HOSTINGER_API_TOKEN` |
| SSH Command | `ssh root@72.62.86.63` |

### Infomaniak (DNS/Hosting)
| Item | Secret |
|---|---|
| API Token | `$INFOMANIAK_API_TOKEN` |

### Services on VPS (72.62.86.63)

| Service | URL | Auth |
|---|---|---|
| DeerFlow | research.garzaos.cloud | None |
| Agent Zero | agentzero.garzaos.cloud | admin / `$AGENT_ZERO_PASSWORD` |
| Perplexica | search.garzaos.cloud | None |
| FreeScout | support.nomad-os.cloud | jadengarza@pm.me / `$FREESCOUT_ADMIN_PASSWORD` |
| Bitwarden Vault | vault.garzaos.cloud | Admin: `$VAULT_ADMIN_TOKEN` |
| Vault MCP | vault.garzaos.cloud/mcp | Bearer: `$VAULT_MCP_BEARER_TOKEN` |
| ProtonMail MCP | protonmail.garzaos.cloud/mcp | Bearer: `$PROTONMAIL_MCP_TOKEN` |
| MCP Toolkit | mcp.garzaos.cloud/mcp | None |
| MCP Factory | mcpfactory.garzaos.cloud/mcp | None |
| MCP Factory Backup | mcpfactory-backup.garzaos.cloud/mcp | None |
| MCP Factory Nomad | mcpfactory-nomad.garzaos.cloud/mcp | None |
| Akaunting | akaunting.garzaos.cloud | jadengarza@pm.me / `$VPS_ROOT_PASSWORD` |
| Invio | invio.garzaos.cloud | admin / `$INVIO_PASSWORD` |
| Sim Studio Fleet | auto1–auto5.garzaos.cloud | Per-instance |
| SuperGlue | superglue.garzaos.cloud | Bearer: `$SUPERGLUE_AUTH_TOKEN` |
| Paperclip | localhost:3100 (local) | None |

---

## 4. MCP Server Auth

### Production MCP Endpoints

| MCP Server | URL | Auth Method | Key / Token |
|---|---|---|---|
| **Garza MCP (Unified)** | mcp.garzaos.cloud/sse | Bearer token | `MCP_AUTH_TOKEN` env var on server |
| **Vault MCP** | vault.garzaos.cloud/mcp | Bearer | `$VAULT_MCP_BEARER_TOKEN` |
| **ProtonMail MCP** | protonmail.garzaos.cloud/mcp | Bearer | `$PROTONMAIL_MCP_TOKEN` |
| **Composio MCP** | connect.composio.dev/mcp | x-consumer-api-key header | `$COMPOSIO_MCP_API_KEY` |
| **MCP Toolkit** | mcp.garzaos.cloud/mcp | None | — |
| **Nextcloud MCP** | mcp-next.garzaos.online/mcp | Optional token | — |
| **Kuse MCP** | kuse-mcp-server.fly.dev/mcp | None | — |

### Legacy / Retiring MCP Endpoints

| MCP Server | URL | Auth (query param) |
|---|---|---|
| Garza Home MCP | garza-home-mcp.fly.dev/sse | `key=$GARZA_HOME_MCP_KEY` (query param) |
| Last Rock Dev | lastrock-mcp.garzahive.com/sse | `key=$LASTROCK_MCP_KEY` (query param) |
| CF MCP | mcp-cf.garzahive.com/sse | `key=$CF_MCP_KEY` (query param) |
| Garza Hive MCP | mcp.garzahive.com/sse | `key=$GARZA_HIVE_MCP_KEY` (query param) |
| SSH Backup | ssh-backup2.garzahive.com/sse | `key=$SSH_BACKUP_MCP_KEY` (query param) |
| Telnet Backup | ssh-backup.garzahive.com/sse | `key=$TELNET_BACKUP_MCP_KEY` (query param) |
| Rube | rube.app/mcp | Bearer JWT (see garza-os configs) |
| Nango (OpenManus) | api.nango.dev/mcp | Bearer: `$NANGO_BEARER_TOKEN` |
| OpenManus Hybrid | openmanus-mcp-production.up.railway.app/sse | Bearer: `$OPENMANUS_BEARER_TOKEN` |

### Garza MCP (Unified) — Environment Variables
The unified MCP server at `mcp.garzaos.cloud` requires these env vars:

| Env Var | Purpose |
|---|---|
| `MCP_AUTH_TOKEN` | Bearer token for all MCP clients |
| `PROTONMAIL_USERNAME` | jadengarza@pm.me |
| `PROTONMAIL_PASSWORD` | Proton Bridge password |
| `IMAP_HOST` / `IMAP_PORT` | 127.0.0.1:1143 (Proton Bridge) |
| `SMTP_HOST` / `SMTP_PORT` | 127.0.0.1:1025 (Proton Bridge) |
| `BEEPER_TOKEN` | Beeper Desktop API token |
| `FABRIC_API_KEY` | Fabric AI API key |
| `QUO_API_KEY` | OpenPhone/Quo API key |
| `VOICENOTES_TOKEN` | VoiceNotes API token |
| `NEXTCLOUD_USERNAME` | admin |
| `NEXTCLOUD_PASSWORD` | Nextcloud app password |
| `NEXTCLOUD_URL` | https://next.garzaos.online |

---

## 5. Nextcloud

| Item | Value / Secret |
|---|---|
| Instance URL | https://next.garzaos.online |
| Admin Username | `$NEXTCLOUD_USERNAME` (admin) |
| Admin App Password | `$NEXTCLOUD_APP_PASSWORD` |
| MCP Server | mcp-next.garzaos.online/mcp |
| Infomaniak Server | Nextcloud hosting via Infomaniak |

### Nextcloud MCP Server Deployment Modes
(from `nextcloud-mcp-server` repo)

| Mode | Auth Method | Key Env Vars |
|---|---|---|
| Single-User | Static credentials | `NEXTCLOUD_USERNAME`, `NEXTCLOUD_PASSWORD` |
| Multi-User BasicAuth | Per-request HTTP Basic | `ENABLE_MULTI_USER_BASIC_AUTH=true`, `TOKEN_ENCRYPTION_KEY` |
| Login Flow v2 | Browser-based OAuth | `ENABLE_LOGIN_FLOW=true`, `TOKEN_ENCRYPTION_KEY` |
| Keycloak | External IdP (OIDC) | `OIDC_DISCOVERY_URL`, `NEXTCLOUD_OIDC_CLIENT_ID`, `NEXTCLOUD_OIDC_CLIENT_SECRET` |

### Dev Docker Defaults (nextcloud-mcp-server)
- Nextcloud admin: `admin` / `admin`
- MariaDB root: `root` / `password`
- MariaDB user: `nextcloud` / `password`
- Keycloak admin: `admin` / `admin`
- Qdrant API key: `my_secret_api_key` (dev default)

---

## 6. Communication Services

### ProtonMail
| Item | Value |
|---|---|
| Email | jadengarza@pm.me |
| Bridge IMAP | 127.0.0.1:1143 (on VPS) |
| Bridge SMTP | 127.0.0.1:1025 (on VPS) |
| Bridge Password | `PROTONMAIL_PASSWORD` env var (on VPS) |
| MCP Token | `$PROTONMAIL_MCP_TOKEN` |

### Beeper
| Item | Value |
|---|---|
| API URL | http://localhost:23373 (local on VPS) |
| API Token | `BEEPER_TOKEN` env var (on VPS) |
| DB Path | /Users/customer/Library/Application Support/BeeperTexts/index.db |
| DB Size | 17GB, 8.3M+ messages |

### Telegram Bots
| Bot | Token Env Var | Used By |
|---|---|---|
| Jada Telegram | `TELEGRAM_BOT_TOKEN` | jada-agent backend |
| Home Engine Bot | `TELEGRAM_BOT_TOKEN` | garza-home-engine |
| Blinko Bot | `TELEGRAM_BOT_TOKEN` | blinko-telegram container |

---

## 7. Vault & Secret Management

### Bitwarden / Vaultwarden
| Item | Value / Secret |
|---|---|
| Vault URL | vault.garzaos.cloud |
| Admin Token | `$VAULT_ADMIN_TOKEN` |
| MCP Endpoint | vault.garzaos.cloud/mcp |
| MCP Bearer | `$VAULT_MCP_BEARER_TOKEN` |

### Vaultwarden Decrypted MCP (Railway)
| Item | Value |
|---|---|
| Bitwarden Server | vaultwarden-production-0d25.up.railway.app |
| Client ID | user.2f293b33-d36e-4f43-a65f-d64d1681732a |
| Client Secret | (stored in Railway env) |
| Master Password | (stored in Railway env) |
| API Key | (generated, stored in Railway env) |

### Fabric AI (Primary Memory Store)
| Item | Value |
|---|---|
| API URL | https://api.fabric.so |
| API Key | `$FABRIC_API_KEY` |
| Project Folder | 89cd201a-0be0-47f2-a25e-bdc1f85c1ef8 |

### Blinko (Legacy — replaced by Fabric)
| Item | Value |
|---|---|
| URL | jadennotes.pikapod.net |
| API Token | `$BLINKO_API_TOKEN` |

---

## 8. Nomad Internet / Business Services

### Chargebee (Billing)
| Item | Secret |
|---|---|
| Site | `$CHARGEBEE_SITE` (nomad-internet) |
| API Key | `$CHARGEBEE_API_KEY` |

### Verizon ThingSpace (M2M/IoT)
| Item | Secret |
|---|---|
| Username | `$VERIZON_USERNAME` |
| Password | `$VERIZON_PASSWORD` |
| Basic Auth | `$VERIZON_BASIC_AUTH` |
| Client Secret | `$VERIZON_SECRET_KEY` |
| Token URL | `$VERIZON_TOKEN_URL` |
| Base URL | https://thingspace.verizon.com/api/m2m/v1 |

### Nomad Customer Bridge (nomad-customer-bridge)
Required env vars for the customer sync service:

| Env Var | Purpose |
|---|---|
| `NOTION_TOKEN` | Notion API (customer registry) |
| `NOTION_DB_ID` | Notion database ID |
| `STRIPE_SECRET_KEY` | Stripe payments API |
| `CHARGEBEE_SITE` | Chargebee site |
| `CHARGEBEE_API_KEY` | Chargebee API |
| `THINKSPACE_CLIENT_ID` | ThingSpace OAuth client |
| `THINKSPACE_CLIENT_SECRET` | ThingSpace OAuth secret |
| `THINKSPACE_USERNAME` | ThingSpace login |
| `THINKSPACE_PASSWORD` | ThingSpace login |
| `SHOPIFY_STORE` | nomad-internet.myshopify.com |
| `SHOPIFY_ACCESS_TOKEN` | Shopify admin API |
| `SHIPSTATION_API_KEY` | ShipStation API key |
| `SHIPSTATION_API_SECRET` | ShipStation API secret |

### Nomad Contact Engine (nomad-contact-engine)
| Env Var | Purpose |
|---|---|
| `NEXTCLOUD_MCP_URL` | Nextcloud MCP endpoint |
| `NEXTCLOUD_TOKEN` | Nextcloud MCP auth |
| `COMPOSIO_MCP_API_KEY` | Gmail/Calendar via Composio |
| `FREESCOUT_URL` | support.nomad-os.cloud |
| `FREESCOUT_EMAIL` | jadengarza@pm.me |
| `FREESCOUT_PASSWORD` | FreeScout API password |
| `BEEPER_MCP_URL` | Beeper MCP endpoint |
| `BEEPER_MCP_TOKEN` | Beeper MCP auth |

### FreeScout (Help Desk)
| Item | Value |
|---|---|
| URL | https://support.nomad-os.cloud |
| Admin Email | jadengarza@pm.me |
| Admin Password | `$FREESCOUT_ADMIN_PASSWORD` |
| MCP API Key | `$FREESCOUT_MCP_API_KEY` |
| Departments (11) | support, sales, billing, cancellations, compliance, neworders, orders, shipping, activations, escalations, legal |

### Akaunting (Accounting)
| Instance | URL | Auth |
|---|---|---|
| Self-hosted | akaunting.garzaos.cloud | jadengarza@pm.me / `$VPS_ROOT_PASSWORD` |
| Cloud | akaunting.com | jadengarza@pm.me / `$AKAUNTING_CLOUD_PASSWORD` |
| Cloud API Key | — | `$AKAUNTING_CLOUD_API_KEY` |

---

## 9. App Logins (Web UIs)

| App | URL | Username | Password Secret |
|---|---|---|---|
| Agent Zero | agentzero.garzaos.cloud | admin | `$AGENT_ZERO_PASSWORD` |
| Invio | invio.garzaos.cloud | admin | `$INVIO_PASSWORD` |
| Akaunting (cloud) | akaunting.com | jadengarza@pm.me | `$AKAUNTING_CLOUD_PASSWORD` |
| Kuse | app.kuse.ai | jadengarza@pm.me | `$KUSE_PASSWORD` |
| FreeScout | support.nomad-os.cloud | jadengarza@pm.me | `$FREESCOUT_ADMIN_PASSWORD` |
| Nextcloud | next.garzaos.online | admin | `$NEXTCLOUD_APP_PASSWORD` |
| SuperGlue | superglue.garzaos.cloud | garzasecure@pm.me | Bearer token auth |

---

## 10. Railway Deployments

| Project | URL | Railway Project ID |
|---|---|---|
| Sim Studio 2 | sim2.garza-os.com | ef478b97-e98f-4cb5-9155-ac5f7ffd50c4 |
| Jaden Auto | auto.garza-os.com | 0a9e19e1-13c6-46c6-b3d7-cf7d06f75224 |
| Paperclip AI | paperclip.garza-os.com | 973eebb8-79aa-4fcd-a488-2eb9c4624049 |
| Coworker | coworker-production-8d66.up.railway.app | 2fb6e3af-6fe3-4d92-9762-05338ad4f550 |

**Railway API Token**: `$RAILWAY_API_TOKEN`

### Railway Service-Specific Env Vars

**Garza Orchestrator** (Railway):
- `RAILWAY_API_TOKEN` — for managing other Railway services
- Standard Node.js service, nixpacks build

**ProtonMail MCP Railway** (protonmail-mcp-railway):
- `PROTONMAIL_MCP_TOKEN` — Bearer auth
- Deployed on Railway with nixpacks

**Vaultwarden Decrypted MCP** (Railway):
- `API_KEY`, `BW_SERVER`, `BW_CLIENT_ID`, `BW_CLIENT_SECRET`, `BW_PASSWORD`
- Docker-based deployment

---

## 11. Repo-Specific Env Vars

### jada-agent (Hermes Backend)
```
OPENROUTER_API_KEY=        # Required — LLM API
COMPOSIO_MCP_API_KEY=      # MCP tools
VAULT_MCP_URL=             # Bitwarden MCP
VAULT_MCP_BEARER_TOKEN=    # Bitwarden auth
PROTONMAIL_MCP_URL=        # ProtonMail MCP
PROTONMAIL_MCP_TOKEN=      # ProtonMail auth
BEARER_TOKEN=              # Hermes API auth
MODEL=                     # LLM model selection
TELEGRAM_BOT_TOKEN=        # Optional Telegram bot
```

### sim (Sim Studio)
```
DATABASE_URL=              # PostgreSQL connection
BETTER_AUTH_SECRET=        # Auth secret (openssl rand -hex 32)
BETTER_AUTH_URL=           # Auth callback URL
ENCRYPTION_KEY=            # Env var encryption (openssl rand -hex 32)
INTERNAL_API_SECRET=       # Internal API route encryption
API_ENCRYPTION_KEY=        # API key encryption
RESEND_API_KEY=            # Optional email provider
ADMIN_API_KEY=             # Optional admin API for GitOps
```

### superglue
```
AUTH_TOKEN=                         # API auth token
NEXT_PUBLIC_SUPERGLUE_API_KEY=     # Frontend API key (same as AUTH_TOKEN)
POSTGRES_USERNAME / PASSWORD=       # Database creds
MASTER_ENCRYPTION_KEY=              # Credential encryption at rest
LLM_PROVIDER=                       # ANTHROPIC / OPENAI / GEMINI / etc.
GEMINI_API_KEY=                     # If using Gemini
OPENAI_API_KEY=                     # If using OpenAI
ANTHROPIC_API_KEY=                  # If using Anthropic
MINIO_ROOT_USER / PASSWORD=         # MinIO file storage
TAVILY_API_KEY=                     # Web search (optional)
LANGFUSE_SECRET_KEY / PUBLIC_KEY=   # LLM observability (optional)
```

### paperclip
```
DATABASE_URL=              # PostgreSQL (or PGlite for dev)
PORT=3100                  # API port
```

### garza-home-engine
```
OPENAI_API_KEY=            # GPT-4o for agent
HOME_ENGINE_KEY=           # API auth key
TELEGRAM_BOT_TOKEN=        # Telegram bot
TELEGRAM_ALLOWED_USERS=    # Comma-separated user IDs
```

### nextcloud-mcp-server
```
NEXTCLOUD_HOST=            # Nextcloud instance URL
NEXTCLOUD_USERNAME=        # Admin username
NEXTCLOUD_PASSWORD=        # Admin password / app password
TOKEN_ENCRYPTION_KEY=      # Fernet key for token storage
OIDC_DISCOVERY_URL=        # For Keycloak mode
NEXTCLOUD_OIDC_CLIENT_ID=  # OAuth client ID
NEXTCLOUD_OIDC_CLIENT_SECRET=  # OAuth client secret
QDRANT_API_KEY=            # Vector DB auth (optional)
OPENAI_API_KEY=            # Embeddings (optional)
OLLAMA_BASE_URL=           # Local embeddings (optional)
TS_AUTHKEY=                # Tailscale auth key (for Claude funnel)
```

---

## 12. Legacy / DigitalOcean Infrastructure

### Server Registry (from garza-os/infra/ip-registry.json)

| Server | IP | Purpose |
|---|---|---|
| garzahive-01 | 192.241.139.240 | Primary MCP, n8n, automation |
| ssh-bastion | 143.198.190.20 | SSH relay backup |
| ssh-redundancy | 159.89.232.130 | Secondary SSH relay |
| garza-n8n | 146.190.157.249 | Dedicated n8n + Postgres + Redis |
| mac-mini | ssh.garzahive.com (tunnel) | Dev workstation, Home Assistant, Proton Bridge |

### Fly.io Apps
| App | Region | Purpose |
|---|---|---|
| garza-home-mcp | sjc/iad | Home automation MCP |
| garza-beeper-intel | iad | Beeper sync worker |
| garza-ears | iad | Voice memo processor |
| computer-use-mcp | iad | Computer use tools |

### n8n Cloud
- URL: garzasync.app.n8n.cloud
- Multiple active workflows (Garza Ears, Shield, Watch, Task Router, etc.)

### Supabase
- Project: garza-os-vault (ref: vbwhhmdudzigolwhklal)
- Tables: secrets, audit_log, beeper_messages, beeper_chats, voice_memos, contacts

---

## 13. Third-Party Integrations

| Service | Auth Type | Where Stored |
|---|---|---|
| Composio | API key header | `$COMPOSIO_MCP_API_KEY` |
| Rube | JWT Bearer | garza-os .mcp.json |
| Nango | Bearer token | garza-os configs |
| Zapier MCP | URL-embedded auth | SYSTEM-STATE.yml |
| Cloudflare Workers | Wrangler auth | Local config |
| Craft Docs | MCP URL auth | garza-os configs |
| Doppler | Railway env vars | Railway project |
| Stripe | Secret key | nomad-customer-bridge env |
| Shopify | Access token | nomad-customer-bridge env |
| ShipStation | API key + secret | nomad-customer-bridge env |
| Notion | Integration token | nomad-customer-bridge env |
| Paperclip Coworker | API token | `$PAPERCLIP_COWORKER_API_TOKEN` |

---

## 14. Credential Rotation Log

| Date | Credential | Action | Reason |
|---|---|---|---|
| — | — | — | No rotations recorded yet |

### Rotation Policy (Recommended)
- **API Keys**: Rotate every 90 days
- **App Passwords**: Rotate on suspected exposure
- **MCP Tokens**: Rotate when changing server deployments
- **VPS Root Password**: Rotate quarterly
- **OAuth Client Secrets**: Rotate with major version deployments

---

## Notes

- **Primary credential source**: Devin Secrets (`${SECRET_NAME}`)
- **Backup source**: Bitwarden Vault at vault.garzaos.cloud
- **Legacy source**: Craft Document 7061 (being migrated)
- **Memory/notes**: Fabric AI replaced Blinko as of April 2026
- **Never store credentials in GitHub repos** — use env vars and secret managers
