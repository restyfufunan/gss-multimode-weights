# gss-mode-weights

Replication code for:

> Fufunan, R., Freese, J., & Olive [last name]. "Mode Effects and Temporal Continuity in the General Social Survey: Mode-Specific Weights for 2022 and 2024." SocArXiv. [link forthcoming]

## Overview

The GSS transitioned to multimode data collection in 2022, combining face-to-face/phone (FTF/Phone) and web administration. This repository provides code to:

1. Construct **mode-specific post-stratification weights** for the 2022 and 2024 GSS waves, enabling researchers to analyze FTF/Phone and web respondents separately.
2. Document **demographic composition differences** between modes.
3. Estimate **mode gaps** in substantive survey responses before and after demographic adjustment.
4. Assess **temporal continuity** — how closely mode-specific estimates track the 2016–2018 pre-multimode baseline.

## Repository Structure

```
gss-mode-weights/
├── 00_setup.R                    # Shared constants, file paths, demographic recode function
├── 01_data_preparation.qmd       # Variable binarization and recode; exports analysis RDS
├── 02_weights.qmd                # Mode-specific post-stratification weights via raking
├── 03_composition.qmd            # Demographic composition by mode (Figures 1–2)
├── 04_mode_gaps.qmd              # Bivariate and adjusted mode gaps (Table 1, Figure 3)
├── 05_persistence.qmd            # Comparison to 2016–2018 baseline (Figures 4–6)
├── 06_completion_mode.qmd        # Completion mode vs. initial contact mode analysis
├── appendix_forest_plots.qmd     # Standalone appendix forest plot regeneration
├── data/
│   └── README.md                 # Data download instructions
└── outputs/
    ├── figures/                  # Generated figures (.png)
    ├── tables/                   # Generated tables (.png)
    └── data/                     # Intermediate data files (.csv)
```

## Data

This repository does not include GSS data files, which cannot be redistributed. To replicate the analysis:

1. Download the **GSS Cumulative Data File (1972–2024), Release 2** from the NORC GSS website:  
   [https://gss.norc.org/get-the-data/stata](https://gss.norc.org/get-the-data/stata)
2. Place the `.dta` file in the `data/` directory and name it `gss7224_r2.dta`.
3. The file path is defined in `00_setup.R` as `GSS_DTA_PATH`.

## Requirements

All analysis is conducted in **R** using **Quarto** (`.qmd`) documents.

### R Packages

```r
install.packages(c(
  "tidyverse",
  "haven",
  "survey",
  "VIM",
  "broom",
  "gt",
  "dominanceanalysis",
  "scales",
  "forcats",
  "knitr"
))
```

### Software

- R ≥ 4.3.0
- Quarto ≥ 1.4
- RStudio (recommended) or VS Code with Quarto extension

## Replication Order

Run files in the following order. Each file sources `00_setup.R` and depends on outputs from prior steps.

| Step | File | Output |
|------|------|--------|
| 1 | `00_setup.R` | (sourced by all files) |
| 2 | `01_data_preparation.qmd` | `gss_data_16to24_r2.rds` |
| 3 | `02_weights.qmd` | `data/gss20222024_modeweights.dta` |
| 4 | `03_composition.qmd` | Figures 1–2 |
| 5 | `04_mode_gaps.qmd` | Table 1, Figure 3 |
| 6 | `05_persistence.qmd` | Figures 4–6, intermediate CSVs |
| 7 | `06_completion_mode.qmd` | Completion mode analysis |
| 8 | `appendix_forest_plots.qmd` | Appendix figures (reads CSVs from step 6) |

## Key Methodological Notes

- **Mode-specific weights** are constructed by raking each mode subsample independently to the full-sample `wtssps`-weighted marginals on seven demographic dimensions: age, sex, education, marital status, nativity, region, and race/ethnicity (8-category Hispanic + Census race classification).
- **Regression controls** use the same 8-category race/ethnicity variable (`w_hisp_race`) as the raking procedure, ensuring consistency between the weighting and modeling stages.
- **Outcome variables** are binarized versions of GSS items, constructed in `01_data_preparation.qmd`. Voting and election items (`vote16`, `vote20`, `pres16`, `pres20`, `if16who`, `if20who`) are excluded from all outcome analyses as they are confounded with election year.
- **Multiple comparison correction**: Holm-Bonferroni correction is applied to mode effect p-values across all outcome variables.

## Citation

If you use these weights or replication code, please cite the paper above and this repository:

> Fufunan, R., Freese, J., & Olive [last name]. (2025). gss-mode-weights [Software]. GitHub. https://github.com/[username]/gss-mode-weights

## License

Code: [MIT License](LICENSE)  
Paper: CC-BY 4.0
