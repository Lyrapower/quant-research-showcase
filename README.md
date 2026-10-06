# Quant Research Platform — Showcase

A paper-trading research platform for discovering, testing and retiring equity and options signals with point-in-time discipline. This repository is a **showcase**: it documents the design, the gates and real outputs. The engine source is private.

**No live trading.** The platform has no order-routing interface. Every signal is settled against real market quotes in a shadow ledger, never against a broker.

## What it does

| Layer | What it is |
|---|---|
| Factor workshop | Proposes factors, compiles them in a sandbox, evaluates them, and keeps a lineage tree of every attempt, including the dead ones and why they died |
| IC evaluator | Daily cross-sectional Spearman IC against h-day forward returns, with t-stat, IR, hit rate and 20-day rolling IC |
| Faultline scanner | Scores where an options-driven price dislocation is building (gamma flip, open-interest walls, gaps, IV term-structure inversion) |
| Shadow ledger | Settles every published card against real quotes at three horizons, so the system grades itself |

Stack: Docker Compose · FastAPI (API + worker) · React/Vite dashboard with three tabs: Ops, Alpha Workshop, Particle Field.

## Why it is different

- **Gates before compute.** A factor with no written hypothesis is dead on arrival. Fewer than 60 observations means it is recorded only, never promoted. At most 12 live factors at any time.
- **Point-in-time or nothing.** Fundamentals become usable on the SEC `acceptedDate` + 1 day. Snapshots are append-only. Options open interest is treated as T+1.
- **Honest settlement.** Close-to-close returns and option-mid settlement are kept as separate tracks so one cannot flatter the other.
- **Self-grading.** The shadow ledger caught the first card's suggested entry at $1.04, about 12% above the actual mid. That gap is now a tracked metric instead of a hidden cost.

## Docs

- [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md): services, data flow, sandbox
- [docs/METHODS.md](docs/METHODS.md): factor gates, IC evaluation, test constitution, faultline heat
- [docs/SAMPLE_OUTPUT.md](docs/SAMPLE_OUTPUT.md): output shapes and the first shadow-ledger finding

## Status

Research and paper trading only. Data: a paid fundamentals/price feed plus an options data feed (v3 API).

## Rights

See [NOTICE](NOTICE). Documentation shared for review. The engine source is not included and not licensed.
