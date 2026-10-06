# Architecture

```
            ┌────────────── React / Vite dashboard ───────────────┐
            │  Ops  ·  Alpha Workshop  ·  Particle Field          │
            └───────────────────────┬─────────────────────────────┘
                                    │ HTTP
                          ┌─────────▼─────────┐
                          │  FastAPI  (:8600) │
                          └───┬───────────┬───┘
                   enqueue    │           │  read
                     ┌────────▼───┐   ┌───▼──────────────┐
                     │   worker   │   │ factors / evals  │
                     │  sandbox   │──▶│ lineage tables   │
                     └────────┬───┘   └──────────────────┘
                              │
              ┌───────────────▼────────────────┐
              │ point-in-time data layer       │
              │ append-only snapshots          │
              │ fundamentals @ acceptedDate+1  │
              │ options OI @ T+1               │
              └────────────────────────────────┘
```

## Services

- **api**: FastAPI on port 8600. Serves the dashboard, factor registry, evaluations and ledger.
- **worker**: runs proposals through the sandbox and evaluator, and writes results to the lineage tables.
- **dashboard**: React/Vite, three tabs.

## Factor lineage

Two core tables: `factors` and `evals`. Each factor records:

- its written hypothesis (required)
- a normalized expression hash, so duplicates are rejected however they are spelled
- its parent factor, if it was derived from one
- its status (`proposed`, `live`, `watch`, `record_only`, `dead`) and, when dead, the reason

Dead branches are kept on purpose. The tree records what was tried and why it failed, so the same idea is not paid for twice.

## Two-stage sandbox

1. **Safety check** on 8 names: the expression must compile, finish, and return finite values.
2. **Full run** on up to 500 names, with a 120-second timeout.

A factor that fails stage 1 never touches the full universe.

## Settlement

Settlement is required before a factor can be judged. Close-to-close equity settlement and option-mid settlement are separate tracks.
