# Cookie Cats A/B Test

Does moving the first gate in the mobile game *Cookie Cats* from level 30 to level 40 improve engagement and retention?
This notebook analyses a randomised experiment with **90,189 players** to find out.

## Result

| Metric | Gate at 30 | Gate at 40 | Difference | p (any difference) |
|---|---|---|---|---|
| Game rounds, first 14 days (mean) | 51.3 | 51.3 | −0.04 rounds | 0.051 (Mann–Whitney) |
| 1-day retention | 44.8% | 44.2% | −0.59 pp | 0.074 |
| **7-day retention** | **19.0%** | **18.2%** | **−0.82 pp** (95% CI −1.33 to −0.31) | **0.0016** |

- None of the targets (+5 rounds, +2 pp day-1 retention, +5 pp day-7 retention) was reached.
- Moving the gate to level 40 **significantly lowered 7-day retention**.
- **Recommendation: keep the first gate at level 30.**

## Method

1. **Data checks:** duplicates, missing values, and a sample-ratio-mismatch test on the 50/50 split.
2. **Outliers:** no IQR trimming, since that would remove the most engaged players and bias retention. One impossible value
   (49,854 rounds in 14 days) is excluded from the game-rounds analysis only.
3. **Power:** minimum detectable effect for the collected sample (≈ 0.9 pp for day-1, ≈ 0.7 pp for day-7 retention),
   confirming the experiment could detect the target effects.
4. **Tests**, all on the full dataset:
   - Game rounds: bootstrap confidence interval for the difference in means, plus a Mann–Whitney U test.
   - Retention: two-proportion z-tests, both two-sided (any difference) and one-sided against the target uplift,
     with 95% confidence intervals.
   - Bootstrap check of both retention differences.
5. **Summary, recommendation and limitations.**

Targets are read as absolute percentage points; α = 0.05, power = 0.80.

## Run it

```bash
pip install -r requirements.txt
jupyter notebook cookie-cats-a-b-testing.ipynb
```

## Data

`cookie_cats.txt` (from the public Cookie Cats dataset on Kaggle):

| Column | Description |
|---|---|
| `userid` | Unique player ID |
| `version` | `gate_30` (control) or `gate_40` (treatment) |
| `sum_gamerounds` | Game rounds played in the first 14 days after install |
| `retention_1` | Player returned 1 day after install |
| `retention_7` | Player returned 7 days after install |
