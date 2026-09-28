# Data

The trial data are **not included** in this repository. They come from the Pan-African Soybean Variety Trials (PAT) network (Feed the Future Soybean Innovation Lab and partners) and were provided for this research project.

The pipeline expects a CSV with one row per genotype per trial and these columns (among others):

| Column | Meaning |
|---|---|
| `COUNTRY`, `YEAR`, `SEASON` | Country, season year, season (1 = dry, 2 = rainy) |
| `env`, `loc`, `gen` | Trial, location and genotype codes |
| `SOWING`, `HARVEST` | Dates (month/day/year or day/month/year; detected automatically) |
| `LAT`, `LON`, `ELEV` | Trial coordinates and elevation |
| `RAINFED` | Water regime (Rainfed, Irrigation, Supplementary) |
| `FLW_DAYS`, `NDM` | Days to flowering (R1) and to maturity (R8) |
| `R6_RUST_Se` | Rust severity at R6 (1–5) |
| `R6_*_Sev` | Other disease severities at R6 |

A public version of the PAT data is described by Araújo et al. (2025), *Scientific Data* 12, 1908, and available on Figshare: https://doi.org/10.6084/m9.figshare.30257359

Place the CSV next to the notebook, or upload it when Colab asks.
