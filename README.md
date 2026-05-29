# STAI-X Challenge — Data Repository

Personal data workspace for the [STAI-X Challenge 2026](https://statsupai.org/STAIX2026/challenge.html): forecasting **state-level rates of suspected nonfatal drug overdose emergency department (ED) visits** using public health, environmental, digital-behavior, and syndromic surveillance signals.

**Author:** [xiyingchen](https://github.com/xiyingchen)

## Challenge overview

The STAI-X 2026 data challenge asks teams to build integrated statistical and AI solutions for timely overdose surveillance. Participants may compete in three award tracks (Leaderboard Excellence, AI Automation, and Statistical Agents). See the [official challenge page](https://statsupai.org/STAIX2026/challenge.html) for registration, deadlines, and submission rules.

| Item | Detail |
|------|--------|
| Task | Forecast overdose-related ED visit rates by state and substance category |
| Geography | U.S. states and jurisdictions (51 in the training covariates) |
| Official platform | Kaggle (competition link on the challenge site) |
| Registration deadline | 2026-06-01 23:59 UTC |
| Submission deadline | 2026-06-15 23:59 UTC |

## Repository layout

```
data/
└── train/
    ├── dose_sys_train.csv          # Training targets (labeled rates)
    ├── covariates.csv              # Training features and context
    └── images/
        └── mat_density/            # MAT density map PNGs (sidecar features)
            └── {ST}_{PERIOD_ID}.png
```

When you add validation or submission assets locally, a typical full layout is:

```
data/
├── train/          # Labeled training split (this repo)
├── val/            # Holdout covariates (labels withheld on Kaggle)
├── sample_submission.csv
└── Data_Description.md
```

## Files in this repository

### `data/train/covariates.csv`

One row per **jurisdiction × period**. Columns include:

| Column | Description |
|--------|-------------|
| `period_id` | Time-period identifier shared with targets and images |
| `jurisdiction` | Two-letter state/territory code |
| `unemployment_rate` | Unemployment rate |
| `labor_force` | Labor force size |
| `temp_avg_f` | Average temperature (°F) |
| `precip_in` | Precipitation (inches) |
| `gtrends_*` | Google Trends indices (overdose, fentanyl, naloxone, opioid, methamphetamine) |
| `state_doh_release` | Optional state department of health press-release text |

**Shape (current snapshot):** 3,927 rows · 51 jurisdictions · 77 periods

### `data/train/dose_sys_train.csv`

Training labels: suspected nonfatal overdose ED visit **rates** by substance category.

| Column | Description |
|--------|-------------|
| `period_id` | Time-period identifier |
| `jurisdiction` | Two-letter state/territory code |
| `overdose_category` | Substance grouping (see below) |
| `rate_per_10000_ed_visits` | Target rate per 10,000 ED visits |

**Overdose categories:** `all_drugs`, `all_opioids`, `all_stimulants`, `heroin`, `fentanyl`, `cocaine`, `methamphetamine`, `benzodiazepine`

**Shape (current snapshot):** 31,416 rows (51 jurisdictions × 77 periods × 8 categories)

### `data/train/images/mat_density/`

PNG sidecar maps keyed as `{ST}_{PERIOD_ID}.png` (e.g. `AK_jtUOZLP4.png`), aligned with `period_id` and `jurisdiction` in the tables above.

**Current snapshot:** 77 PNG files (Alaska periods in this upload)

## Quick start

```python
import pandas as pd
from pathlib import Path

cov = pd.read_csv("data/train/covariates.csv")
y = pd.read_csv("data/train/dose_sys_train.csv")

# Example: merge features and targets for one category
df = cov.merge(
    y[y["overdose_category"] == "fentanyl"],
    on=["period_id", "jurisdiction"],
    how="inner",
)

# Optional: load a MAT density image for a row
# img_path = Path("data/train/images/mat_density") / f"{row.jurisdiction}_{row.period_id}.png"
```

Build models that predict `rate_per_10000_ed_visits` on held-out periods/jurisdictions, then format predictions to match the competition `submission.csv` template on Kaggle.

## Data use

- Use the **official Kaggle competition dataset** for award submissions and follow all [participant rules](https://statsupai.org/STAIX2026/challenge.html) (no external data, no multi-accounting, reproducible notebooks).
- This GitHub repo is a convenience copy for development; do not treat it as a substitute for the official distribution if rules differ.
- Questions about the challenge: staix.contact@gmail.com

## License

Competition data remain subject to the STAI-X / Kaggle terms. Code and documentation in this repository are provided by the author unless otherwise noted.
