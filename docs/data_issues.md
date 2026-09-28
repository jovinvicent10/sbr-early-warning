# Data-quality findings for the data team

Found while building the model (September 2026). Trial codes refer to the `env` column.

| # | Trial(s) | Finding | Question |
|---|---|---|---|
| 1 | 26 trials (e.g. E0104, E0169–E0176) | Every genotype scored 0 for rust; in 23 of them no other disease was recorded either | Were these trials assessed for rust? Does 0 mean "no rust" or "not assessed"? |
| 2 | E0169–E0176 (Zambia, 2020/21) | All 8 Zambian trials of the season are entirely 0 | Was rust scored in Zambia that season? |
| 3 | L074, L099 (Zambia) | Same locations recorded all 0 in some years and all 1 in others | Did the scoring convention for "no rust" change? |
| 4 | E0230, E0249, E0266 | 0 used where the 1–5 scale expects 1 | Confirm 0 = no symptoms in these trials |
| 5 | E0266 (Malawi) | Sown 26 May 2023 (dry season); no rain and no infection-favourable days before R4, yet 25 genotypes scored exactly 3 | Check sowing date and rust scores |
| 6 | E018 (Malawi) | 23 of 36 records dated sown 21/12/2018 and harvested 25/05/2019 in a 2017 trial | Year typo? (corrected to 2017/2018 in the pipeline) |
| 7 | E0169 (Zambia) | Harvest 12/05/2020 before sowing 26/12/2020 | Year typo? (corrected to 2021 in the pipeline) |
| 8 | E0279 (Zambia) | 17 genotypes harvested 5 March 2024, 74 days after sowing and before estimated maturity; others in April | Should 05/03 be 05/04? |
| 9 | E0200, E024, E079 (Malawi) | Dry-season trials with 201–225 days from sowing to harvest | Confirm harvest dates |
