# Nomad (NMD) — Brand Overview

**Code:** `NMD` · **Primary focus:** Nomad Internet customer-ops stack · **Sessions:** 13 in the Nomad bucket + 1 Nomad-Helpdesk

Nomad is the customer support & internet-ops brand. It wraps FreeScout, Zendesk, a Nomad-specific MCP bridge, and a set of KPI / reporting tools on top of the Garza OS substrate.

## Active repos

| Repo | Purpose | Primary sessions |
|---|---|---|
| [`itsablabla/freescout`](https://github.com/itsablabla/freescout) | FreeScout helpdesk (primary Nomad support UI) | [`f512eab5`](https://app.devin.ai/sessions/f512eab546de438c9d838bfcec9f319f) |
| [`itsablabla/nomad-contact-engine`](https://github.com/itsablabla/nomad-contact-engine) | Contact orchestration / outbound engine (private) | — |
| [`itsablabla/Nomad-KPI-System`](https://github.com/itsablabla/Nomad-KPI-System) | KPI reporting system (private) | — |

## Scaffolded from the 2026-04-19 inventory pass

| Repo | State | Originating session |
|---|---|---|
| [`itsablabla/nomad-customer-bridge`](https://github.com/itsablabla/nomad-customer-bridge) | pre-existing | [`a8007dd7`](https://app.devin.ai/sessions/a8007dd77de74e468263cb26a2bb3c05) |
| [`itsablabla/nomad-zendesk-ops`](https://github.com/itsablabla/nomad-zendesk-ops) | stub README | [`79a539a4`](https://app.devin.ai/sessions/79a539a4904340d4afa69f27f39a37d9) |
| [`itsablabla/nomad-mcp-bridge`](https://github.com/itsablabla/nomad-mcp-bridge) | stub README | [`68af4c1c`](https://app.devin.ai/sessions/68af4c1c7eef4695adc468447dfd1b2a) |

## Credentials / env var convention

Nomad services expect secrets named `NOMAD_*` or `NMD_*`. Zendesk secrets use `ZENDESK_*`. FreeScout secrets use `FREESCOUT_*`.

## Open questions

- Should `nomad-contact-engine` and `Nomad-KPI-System` consume or replace the scaffolded `nomad-customer-bridge` / `nomad-zendesk-ops`? Their scopes overlap.
- Session `Nomad Internet helpdesk tasks` ([`f512eab5`](https://app.devin.ai/sessions/f512eab546de438c9d838bfcec9f319f)) is the only Nomad session that produced a concrete FreeScout-targeted patch; future helpdesk sessions should link back to it.
