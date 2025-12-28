# GARZA OS Context Layer

**READ THIS REPO AT SESSION START**

This repo contains operational context for Claude sessions. Before doing anything complex, fetch `SYSTEM-STATE.yml` and `TOOL-ROUTER.md`.

## Quick Start for Claude

```
1. Fetch SYSTEM-STATE.yml - What's running, what's broken
2. Fetch TOOL-ROUTER.md - Which tool for what task
3. Check KNOWN-ISSUES.md if something fails
```

## Files

| File | Purpose |
|------|---------|
| `SYSTEM-STATE.yml` | Live status of all MCP servers and services |
| `TOOL-ROUTER.md` | Decision tree: task → correct tool |
| `CREDENTIALS-MAP.md` | Where to find credentials (Craft docs, vault keys) |
| `KNOWN-ISSUES.md` | Workarounds for common failures |
| `ACTIVE-PROJECTS.md` | Current work in progress |
| `snippets/` | Reusable code patterns |

## Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    GARZA OS                              │
├─────────────────────────────────────────────────────────┤
│  CORE MCP SERVERS                                        │
│  ├── Craft (Knowledge)                                   │
│  ├── Garza Home MCP (Personal/Home)                      │
│  ├── Last Rock Dev (Infrastructure/Dev)                  │
│  └── Nomad MCP (Business) [Future]                       │
├─────────────────────────────────────────────────────────┤
│  OPTIONAL                                                │
│  ├── Cloudflare Platform (Workers/D1/KV/R2)              │
│  └── Zapier (Carbon Voice, Quo, Tables)                  │
├─────────────────────────────────────────────────────────┤
│  RETIRING (Don't use)                                    │
│  ├── CF MCP                                              │
│  ├── Garza Hive MCP                                      │
│  ├── SSH Back Up                                         │
│  └── Telnet Back Up                                      │
└─────────────────────────────────────────────────────────┘
```

## Repos

| Repo | Purpose | Deploy Target |
|------|---------|---------------|
| `garza-os-context` | This repo - Claude's brain | N/A |
| `garza-home-mcp` | Personal/Home MCP server | Fly.io |
| `lastrock-mcp` | Dev/Infra MCP server | Fly.io |
| `unifi-protect-mcp` | UniFi camera tools | Embedded |

## Owner

Jaden Garza - CEO, Last Rock Labs
