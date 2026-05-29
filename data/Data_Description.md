# Data description

## Training split (`train/`)

### `dose_sys_train.csv`

Labeled training targets for the STAI-X overdose ED rate forecasting task.

- **Unit of observation:** jurisdiction × period × `overdose_category`
- **Target:** `rate_per_10000_ed_visits` — suspected nonfatal overdose ED visit rate per 10,000 ED visits

### `covariates.csv`

Features and contextual signals aligned to jurisdiction × period.

- Socioeconomic: `unemployment_rate`, `labor_force`
- Environment: `temp_avg_f`, `precip_in`
- Digital behavior: `gtrends_overdose`, `gtrends_fentanyl`, `gtrends_naloxone`, `gtrends_opioid`, `gtrends_methamphetamine`
- Text (optional): `state_doh_release` — state health department announcements

### `images/mat_density/`

Raster sidecars named `{ST}_{PERIOD_ID}.png`, where `ST` is the jurisdiction code and `PERIOD_ID` matches `period_id` in the CSV files. Join on `(jurisdiction, period_id)` when using images in a model.

## Identifiers

- `period_id` — opaque string ID for a forecast period (shared across tables and filenames)
- `jurisdiction` — U.S. state or territory abbreviation

## Validation split

Validation covariates and images are distributed through the official Kaggle competition; labels are withheld for scoring.
