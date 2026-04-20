# Jada — Brand Overview

**Code:** *(no formal 3-letter code — "Jada" literally)* · **Primary focus:** Autonomous AI agent platform on Nextcloud, powered by OpenClaw

Jada is the most actively-developed agent platform in the workspace — 5 project-level sessions plus numerous sibling repos that share its runtime (coder, cowork, research).

## Active repos

| Repo | Purpose | Primary sessions |
|---|---|---|
| [`itsablabla/jada-agent`](https://github.com/itsablabla/jada-agent) | Nextcloud app — Jada frontend + orchestration for OpenClaw | 6 sessions |
| [`itsablabla/jada-code`](https://github.com/itsablabla/jada-code) | Jada code-focused variant | 2 sessions |
| [`itsablabla/jada-coder`](https://github.com/itsablabla/jada-coder) | Jada coder harness | 2 sessions |
| [`itsablabla/jada-cowork`](https://github.com/itsablabla/jada-cowork) | Jada collaborative/co-working workspace | 5 sessions |
| [`itsablabla/jada-research`](https://github.com/itsablabla/jada-research) | Jada research branch | 2 sessions |
| [`itsablabla/jada-research-2`](https://github.com/itsablabla/jada-research-2) | Follow-up research experiment | 2 sessions |
| [`itsablabla/kuse_cowork`](https://github.com/itsablabla/kuse_cowork) | Kuse cowork (sibling to jada-cowork) | 4 sessions |

## Architecture (from `jada-agent` autogen note)

Jada = Nextcloud 28+ app (PHP 8.1+) that fronts OpenClaw, an external Docker-based autonomous execution engine. Jada speaks MCP to external tools/data and uses OpenRouter / Anthropic / OpenAI as LLM backends. License: AGPL-3.0.

Key concepts: Skills (modular capability units), Schedules (recurring jobs), Agent Health (Nextcloud↔OpenClaw connectivity), MCP Tools, Identity (agent persona), Activity Feed.

## Credentials / env var convention

- `JADA_*` — Jada-specific
- `OPENCLAW_*` — OpenClaw execution engine
- `OPENROUTER_API_KEY`, `ANTHROPIC_API_KEY`, `OPENAI_API_KEY` — LLM backends
- `NEXTCLOUD_*` — host Nextcloud connection

## Open questions

- `jada-research` vs `jada-research-2`: are these separate long-running branches or is `jada-research-2` meant to replace the original? Session metadata isn't explicit.
- `kuse_cowork` lives outside the Jada namespace but shares the cowork concept; worth deciding whether to rename or keep separate.

## Related brand docs

- [Garza (GRZ)](GARZA.md) · [Nomad (NMD)](NOMAD.md) · [Last Rock Labs (LRL)](LAST-ROCK-LABS.md) · [Full sessions index](../DEVIN-SESSIONS-INDEX.md) · [Active projects](../../ACTIVE-PROJECTS.md)
