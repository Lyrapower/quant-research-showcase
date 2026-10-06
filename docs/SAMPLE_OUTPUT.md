# Sample output

The shapes below show which fields are produced. Values marked `…` are omitted on purpose, and nothing here is a backtest claim.

## Evaluation record

```json
{
  "factor": "reversal_5d_resid",
  "horizon_days": 5,
  "n_obs": "…",
  "min_names_per_day": 20,
  "mean_ic": "…",
  "t_stat": "…",
  "ir": "…",
  "hit_rate": "…",
  "rolling_ic_20d": ["…"],
  "verdict": "keep | watch | insufficient | dead",
  "settlement": "close_to_close"
}
```

## Shadow-ledger card

```json
{
  "card_id": "…",
  "instrument": "option",
  "suggested_entry": 1.04,
  "actual_mid_at_publish": "≈0.93",
  "entry_gap_pct": "≈+12%",
  "settle_horizons": ["h1", "h2", "h3"],
  "settlement": "option_mid"
}
```

**First real finding.** The first published card suggested $1.04, about 12% above the actual mid. The ledger exists to catch this kind of gap. Entry slippage is now measured on every card.
