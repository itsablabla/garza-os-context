# Last Rock Labs (LRL) — Brand Overview

**Code:** `LRL` · **Primary focus:** Billing + invoicing for the Last Rock Labs services business

## Active repos

| Repo | Purpose | Primary sessions |
|---|---|---|
| [`itsablabla/akaunting`](https://github.com/itsablabla/akaunting) | Akaunting finance app | — |
| [`itsablabla/actual`](https://github.com/itsablabla/actual) | Actual Budget (personal finance) | — |

## Scaffolded from the 2026-04-19 inventory pass

| Repo | State | Originating session |
|---|---|---|
| [`itsablabla/lastrock-billing`](https://github.com/itsablabla/lastrock-billing) | stub README | [`eb7bf65a`](https://app.devin.ai/sessions/eb7bf65aa7b548fd9fb85e1aa440f20f) — *priority: critical* |

## Running services

- **Invio Invoicing** at `invio.garzaos.cloud` — originating session [`b13e5906`](https://app.devin.ai/sessions/b13e59065e71403fb388190ce198dc45). No Last-Rock-Labs-specific repo yet; currently runs off upstream image + config in the session transcript.

## Credentials / env var convention

- `CHARGEBEE_*` — Chargebee billing (core of `lastrock-billing`)
- `AKAUNTING_*` — Akaunting instance
- `INVIO_*` — Invio invoicing

## Open questions

- The `lastrock-billing` session is tagged `priority:critical`. This repo should be the first scaffold to get real code — migrating billing from manual Akaunting to Chargebee.

## Related brand docs

- [Garza (GRZ)](GARZA.md) · [Nomad (NMD)](NOMAD.md) · [Jada](JADA.md) · [Full sessions index](../DEVIN-SESSIONS-INDEX.md) · [Active projects](../../ACTIVE-PROJECTS.md)
