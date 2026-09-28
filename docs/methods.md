# Methods

## 1. Trial data

The Pan-African Soybean Variety Trials (PAT) database contains one record per genotype per trial (replicates already averaged). A **trial** (environment, `env`) is one location in one season; each `env` code corresponds to exactly one location × year × season combination.

| Item | Full file | Used (Malawi + Zambia, assessed trials) |
|---|---|---|
| Records | 3,653 | 2,803 |
| Trials | 101 | 78 |
| Genotypes | 175 | 174 (before exclusion) |
| Years | 2017–2024 | 2017–2024 |

The term **genotype** covers both released varieties and experimental breeding lines.

## 2. Cleaning

- Dates are read in the file's own layout (month/day/year in MT4, day/month/year in MT2); the notebook detects the layout that reads every date and stops if it cannot decide.
- Per-record date fixes: sowing later than 31 March of the following season year → sowing and harvest moved back one year (E018); harvest before sowing → harvest +1 year (E0169); harvest more than 300 days after sowing → harvest −1 year.
- Zeros in days to flowering, days to maturity, seed weight and plant height are treated as missing.

## 3. Rust scores and quality control

Rust severity was scored at R6 on the PAT 1–5 scale. The file also contains zeros, which are not on the scale. Four patterns were found: 60 trials without zeros, 26 trials scored entirely 0, 12 with a few zeros among 1s, and 3 that used 0 instead of 1. In 23 of the 26 all-zero trials no other disease was recorded either.

A trial is flagged **probably not assessed** when every genotype scored 0 and no other disease was recorded. In Malawi and Zambia, 21 trials were flagged and excluded; remaining zeros are recoded as 1 (no rust).

## 4. Targets

- **Severity:** mean rust score over all genotypes in the trial (1–5).
- **Outbreak:** at least one genotype scored 3 or more (the site-year criterion of Favoretto et al. 2025).

## 5. Growth stages

Only sowing date, days to flowering (R1) and days to maturity (R8) are recorded. R4 and R6 are placed at the relative positions of the Manitoba Soybean Plant Development Guide (R1 45, R4 70, R6 90, R8 120 days):

- R4 = R1 + 0.33 × (R8 − R1)
- R6 = R1 + 0.60 × (R8 − R1)

Trial medians of flowering and maturity days are used; missing values take the overall median. Median results: R1 49, R4 69, R6 86, R8 110 days after sowing. Estimated R8 fell a median of 26 days before the recorded harvest.

## 6. Weather

Hourly ECMWF IFS (9 km) data were retrieved from the Open-Meteo Historical Weather API for each trial's coordinates (`cell_selection=land`, local time zone). Variables: 2 m temperature, relative humidity, precipitation, shortwave radiation, 10 m wind speed and direction, vapour pressure deficit, and soil moisture (0–7 cm).

## 7. Features (14 days before R4)

| Feature | Definition |
|---|---|
| `wet_hours` | Hours with RH ≥ 90% (leaf-wetness proxy) |
| `wet_days6` | Days with ≥ 6 wet hours (Melching et al. 1989) |
| `infect_days` | Days with ≥ 6 wet hours and mean wet-period temperature 17–29 °C (Favoretto et al. 2025) |
| `tmin`, `tmax` | Mean daily minimum and maximum temperature |
| `hot_days` | Days with maximum temperature > 30 °C |
| `hours_T17_29` | Hours with temperature 17–29 °C |
| `rain_total`, `rain_days` | Total rain; days with ≥ 1 mm (Del Ponte et al. 2006) |
| `rh_mean`, `vpd_mean` | Mean relative humidity; mean vapour pressure deficit |
| `srad` | Mean shortwave radiation (24 h, including night) |
| `wind`, `wind_sin`, `wind_cos` | Mean wind speed; wind direction as circular components |
| `soilm` | Mean soil moisture, 0–7 cm |

## 8. Models and validation

- **Severity:** random forest regressor (500 trees, `min_samples_leaf=4`, `max_features=0.5`), compared with a baseline that always predicts the training mean.
- **Outbreak:** logistic regression with standardised features, `C=0.3`, `class_weight='balanced'`, compared with a prior-probability baseline.
- **Validation:** year-by-year forward chaining. For each test year from 2019, models are trained only on earlier years. 63 trials (2019–2024) are predicted once each.
- **Metrics:** mean absolute error on trials with rust (observed ≥ 1.5) and its reduction relative to the baseline; Spearman correlation; recall, precision and AUC for outbreaks. Class accuracy is not used, because always predicting "no rust" is right for most trials.

## 9. Alerts

Outbreak probability is mapped to four levels: Green (< 0.25), Yellow (0.25–0.50), Orange (0.50–0.75) and Red (≥ 0.75). Thresholds are placeholders to be agreed with plant pathologists.
