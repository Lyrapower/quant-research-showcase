# Methods

## 1. Factor gates

| Gate | Rule |
|---|---|
| Hypothesis | Empty hypothesis → proposal is dead |
| Live cap | At most 12 live factors |
| Sample size | `n_obs < 60` → recorded only, never promoted |
| Settlement | No verdict without settled returns |
| Dedup | Normalized hash; the same expression spelled differently is rejected |
| Lineage | Parent recorded; dead branches kept with reasons |

## 2. IC evaluation

- Daily cross-sectional **Spearman IC** between factor value and h-day forward return (default h = 5 trading days)
- Reported: mean IC, t-stat, IR, hit rate, 20-day rolling IC
- Minimum cross-section: 20 names per day
- Verdicts:
  - `t ≥ 2` → **keep**
  - significant in the reverse direction → **watch** (the sign may be the signal)
  - `n < 60` → **insufficient**

## 3. Test constitution

Rules fixed before any factor is tested:

1. **30% out-of-sample, looked at once.** No second look and no retuning after seeing it.
2. Metrics: RankIC, ICIR, quintile spreads, with a turnover penalty.
3. **Bonferroni-adjusted** threshold: `t ≥ 2.6` when testing a batch.
4. **IC decay slope** across horizons: a real signal decays smoothly, while noise does not.
5. **Redundancy**: if two factors correlate above 0.7, keep one.

## 4. First four factors

| Factor | Idea |
|---|---|
| 5-day residual reversal | Short-term overreaction after removing market and sector moves |
| 12-1 momentum | 12-month return skipping the most recent month |
| 52-week-high proximity | Anchoring near the yearly high |
| Sector relative strength | Name vs. its own sector |

## 5. Faultline heat

A 0–100 score for where an options-driven dislocation is building:

```
H = 100 × (0.35·distance + 0.25·momentum + 0.25·structure + 0.15·volume)
```

Structural flags:

- **F1** gamma flip level
- **F2** open-interest walls
- **F3** gap ≥ 1.5 ATR
- **F4** IV term-structure inversion

## 6. Crowding

Alpha decay is tracked against crowding: a factor whose IC falls as the same trade becomes popular is flagged instead of being averaged away.

## 7. Point-in-time discipline

- Fundamentals usable from SEC `acceptedDate` + 1 day
- Restatements (e.g. 10-K/A) appended as new data points and never overwrite the original
- Options open interest treated as known at T+1
- All snapshots append-only
