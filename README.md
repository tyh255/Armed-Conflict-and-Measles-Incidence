# Replication Code: Armed Conflict and Global Measles Cases
### A Structural Equation Modeling Analysis of 193 Countries, 2000–2023

**Headley TY & Tozan Y** | *PLOS Medicine* (2026)

---

## Overview

This repository contains the R code needed to replicate the primary analyses reported in the above manuscript, including the fixed-effects panel regressions (Table 2), the four primary structural equation models (Table 3), and the first-difference and extended lag robustness analyses (Supporting Tables S10 and S11).

The study examines direct and indirect pathways linking armed conflict, population displacement, and socioeconomic development to measles cases and incidence across 193 countries from 2000 to 2023, using fixed-effects panel regression and structural equation modeling (SEM).

---

## Requirements

### R version
The script was developed and tested on R 4.3.2.

### R packages
Install all required packages before running the script:

```r
install.packages(c(
  "dplyr",
  "tidyr",
  "tidyverse",
  "lavaan",
  "semTools",
  "purrr",
  "plm",
  "lmtest"
))
```

---

## Data Sources

The script expects eight input files, placed in the `./data/` subdirectory. These files are not included in this repository because they are drawn from publicly available institutional sources. Each file can be downloaded directly from the source listed below.

| File name | Source | URL |
|---|---|---|
| `vaccination_thresholds.csv` | WHO/UNICEF Joint Estimates of National Immunization Coverage (WUENIC) | https://immunizationdata.who.int |
| `WHO Measles Incidence.csv` | WHO Immunization Dashboard (reported measles cases) | https://immunizationdata.who.int |
| `WHO_Measles Case Incidence_Per Million.csv` | WHO Immunization Dashboard (measles incidence per million) | https://immunizationdata.who.int |
| `IHME Measles Incidence.csv` | Institute for Health Metrics and Evaluation, Global Burden of Disease Study 2021 | https://ghdx.healthdata.org/gbd-results-tool |
| `hdr-data.csv` | United Nations Development Programme, Human Development Index | https://hdr.undp.org/data-center/human-development-index |
| `WB_Health Expenditures.csv` | World Bank, World Development Indicators (current health expenditure, % of GDP) | https://databank.worldbank.org/source/world-development-indicators |
| `WB_TB Incidence Rate.csv` | World Bank, World Development Indicators (TB incidence per 100,000) | https://databank.worldbank.org/source/world-development-indicators |
| `UNHCR_persons_of_concern.csv` | UNHCR Refugee Data Finder (refugees, asylum seekers, IDPs by country of origin) | https://www.unhcr.org/refugee-statistics/download |

Battle-related deaths data were obtained from the Uppsala Conflict Data Program (UCDP) Georeferenced Event Dataset and are incorporated into `vaccination_thresholds.csv` as the variable `bd_best`. Raw UCDP data are available at https://ucdp.uu.se/downloads.

All data were downloaded as of early 2025 and cover the period 2000–2023.

---

## Running the Script

1. Clone or download this repository.
2. Download the required data files (see above) and place them in the `./data/` subdirectory.
3. Create an empty `./results/` subdirectory to receive output files, or update the write paths in the script.
4. Open `conflict_measles_replication.R` in R or RStudio and run it in full, or section by section.

The script is organized into six sequential sections and must be run in order, as later sections depend on objects created earlier:

| Section | Content | Output file |
|---|---|---|
| 1 | Data preparation and standardization | `semds_std` (in memory) |
| 2 | Helper functions | — |
| 3 | Primary SEM models A–D | `Table3_Primary_SEM.csv` |
| 4 | Fixed-effects panel regressions (Models 1–20) | `Table2_FixedEffects.csv` |
| 5 | First-difference SEM | `S10_FirstDifference_SEM.csv` |
| 6 | Extended 3-year lag models and Wald tests | `S11_ExtendedLag_SEM.csv` |

---

## Analytical Notes

**SEM estimation.** All structural equation models were estimated using the `lavaan` package with the MLR estimator (maximum likelihood with robust Huber-White standard errors), full information maximum likelihood (FIML) for missing data, and standardized latent variable scaling (`std.lv = TRUE`). Standardized path coefficients are reported throughout.

**Socioeconomic development latent variable.** This construct is defined by three observed indicators: GDP per capita (log-transformed, standardized), life expectancy (standardized), and mean years of schooling (standardized). All three load strongly on the construct (standardized loadings 0.83–0.94 across models).

**Variable transformation.** Count and heavily skewed variables (battle-related deaths, measles cases, GDP per capita, TB incidence) were log-transformed using `log1p()` prior to global z-score standardization. Rate and continuous variables were standardized directly. Standardized coefficients can be interpreted as the expected standard deviation change in the outcome per one standard deviation change in the predictor.

**Fixed-effects regressions.** All panel regressions use country fixed effects (`effect = "individual"`) with Arellano/HC1 cluster-robust standard errors clustered at the country level.

**First-difference SEM.** This model operates on year-over-year changes in all variables and is clustered by country. It is reported in Supporting Table S10.

**Extended lag models.** Models E and F include conflict exposure at lags 0 through 3 years. Wald tests assess joint significance of all lag terms simultaneously. These are reported in Supporting Table S11.
