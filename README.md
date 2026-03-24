# Share Swap Alliance Event Study

This repository replicates the empirical analysis from:

> Yeh et al. (2026). *Share Swap-Based Strategic Alliances and Market Reaction: Evidence from Taiwan*. National Taipei University.

The study examines cumulative abnormal returns (CAR) surrounding share swap alliance announcements in Taiwan using event study methodology.


## Research Design

- **Sample**: 8 share swap alliance events (15 firms) listed on TSE or TPEx, 2016–2025
- **Estimation Window**: [-240, -41]
- **Event Windows**: (-1, +1), (-5, +5), (-10, +10)
- **Model**: Market Model (OLS)
- **Data Source**: Taiwan Economic Journal (TEJ)


## Repository Structure

```
event-study/
├── data/
│   ├── AR_tej.xlsx               # AR and SAR exported from TEJ
│   ├── clean_event_data.csv      # Cleaned long-format data (output of 02)
│   ├── ols_data.xlsx             # Foxconn single-firm demo data
│   └── firm_fund.xlsx            # Firm-level fundamentals and CAR for regression
└── notebooks/
    ├── 01_ols_demo.ipynb         # OLS market model demo (Foxconn)
    ├── 02_data_cleaning.ipynb    # Reshape TEJ data to long format
    ├── 03_line_chart.ipynb       # AR and SAR time-series plot
    ├── 04_all_samples_test.ipynb # Full-sample CAR/CSAR significance tests
    ├── 05_subgroup_ttest.ipynb   # Paired t-test and Welch's t-test by subgroup
    └── 06_regression.ipynb       # Cross-sectional OLS regression
```


## Analysis Flow

**1. OLS Demo** (`01_ols_demo.ipynb`)

Demonstrates the market model estimation for a single firm (Foxconn). Estimates alpha and beta from the estimation window, computes AR and SAR, and aggregates CAR/CSAR across event windows.

**2. Data Cleaning** (`02_data_cleaning.ipynb`)

Reshapes TEJ-exported wide-format AR/SAR data into long format for downstream analysis. Outputs `clean_event_data.csv`.

**3. Line Chart** (`03_line_chart.ipynb`)

Plots sample-average AR and SAR across event days to visualize return patterns around announcement dates.

**4. Full-Sample Test** (`04_all_samples_test.ipynb`)

Tests whether mean CAR and CSAR are significantly different from zero across all 15 firms. Uses Z-test (normal distribution), consistent with TEJ methodology.

**5. Subgroup Tests** (`05_subgroup_ttest.ipynb`)

- **Paired t-test**: Compares CAR between leading and partnering firms within each alliance event (7 pairs; SAA–SynPower excluded as SynPower was not publicly listed)
- **Welch's t-test**: Compares CAR between technologically oriented (N=9) and non-tech (N=6) alliances

**6. Cross-Sectional Regression** (`06_regression.ipynb`)

OLS regression of CAR on alliance-level and firm-level characteristics:

| Variable | Description |
|---|---|
| LEAD | 1 if leading firm, 0 if partnering firm |
| TECH | 1 if tech-oriented alliance, 0 otherwise |
| RD | R&D intensity (R&D expenditure / total assets) |
| BM | Book-to-market ratio |
| TECH × RD | Interaction term (RD mean-centered) |


## Requirements

```
pandas
numpy
scipy
statsmodels
matplotlib
openpyxl
```


## Notes

- AR and SAR for all firms were computed by TEJ. `01_ols_demo.ipynb` replicates the methodology for a single firm to verify consistency.
