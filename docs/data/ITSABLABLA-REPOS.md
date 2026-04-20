# `itsablabla/*` — full authenticated repo inventory

Snapshot pulled 2026-04-20 via Composio `GITHUB_LIST_REPOSITORIES_FOR_THE_AUTHENTICATED_USER` (pages 1–3, `per_page=100`).

## Counts

| Visibility | Count |
|---|---|
| **Total unique repos** | **262** |
| Private | 150 |
| Public  | 112 |
| Forks   | 66 |
| Archived | 0 |

## Files in this directory

- [`ITSABLABLA-REPOS.csv`](ITSABLABLA-REPOS.csv) — one row per repo with `full_name, private, archived, fork, default_branch, language, size, pushed_at`.

## Why this exists

The earlier index (`docs/DEVIN-SESSIONS-INDEX.md`, first pass) only saw the ~112 public repos. After the Expand + cleanup passes we re-authenticated and pulled **262 unique repos** (including 150 private ones). That full corpus is what the token-matching inference in `build_index.py` now runs against, so sessions are mapped to the right repo even when it's private.

## Delta vs. previous snapshot

- Previous snapshot (cleanup pass): 259 repos.
- Current snapshot: 262 repos.
- Net new (3): `garza-os-personal-intelligence`, `garza-family-presence-plan`, and `capy-control` appearing as most-recently-pushed.

## Operational notes

- `default_branch` varies — most are `main`, but several older repos (e.g. `garza-school-hub`, `akaunting`, `actual`, many `*-mcp-server` repos) use `master`. Any automation that commits to these repos must branch-switch accordingly.
- `fork=1` repos (66 of them) are tracking upstream projects; don't force-push to those branches.
- `size=0` repos are empty / placeholder (e.g. `coworker-platform` until real code lands).
