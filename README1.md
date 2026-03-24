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
│   ├── AR_tej.xlsx               # Exported from TEJ
│   ├── clean_event_data.csv      # Output of 02
│   ├── ols_data.xlsx             # Foxconn demo data
│   └── firm_fund.xlsx            
└── notebooks/
    ├── 01_ols_demo.ipynb         
    ├── 02_data_cleaning.ipynb    
    ├── 03_line_chart.ipynb       
    ├── 04_all_samples_test.ipynb 
    ├── 05_subgroup_ttest.ipynb   
    └── 06_regression.ipynb       
```


## Analysis Flow

**1. OLS Demo** (`01_ols_demo.ipynb`)

Demonstrates the market model estimation for a single firm (Foxconn). Computes AR and SAR, and aggregates CAR/CSAR across event windows.

**2. Data Cleaning** (`02_data_cleaning.ipynb`)

Reshapes TEJ-exported wide-format AR/SAR data into long format for downstream analysis. Outputs `clean_event_data.csv`.

**3. Line Chart** (`03_line_chart.ipynb`)

Plots sample-average AR and SAR across event days to visualize return patterns around announcement dates.

**4. Full-Sample Test** (`04_all_samples_test.ipynb`)

Tests whether mean CAR and CSAR are significantly different from zero across all 15 firms.

**5. Subgroup Tests** (`05_subgroup_ttest.ipynb`)

- **Paired t-test**: Compares CAR between leading and partnering firms within each alliance event (7 pairs)
- **Welch's t-test**: Compares CAR between technologically oriented (N=9) and non-tech (N=6) alliances

**6. Cross-Sectional Regression** (`06_regression.ipynb`)

OLS regression of CAR on alliance-level and firm-level characteristics:

| Variable | Description |
|---|---|
| LEAD | 1 if leading firm, 0 if partnering firm |
| TECH | 1 if tech-oriented alliance, 0 otherwise |
| RD | R&D intensity |
| BM | Book-to-market ratio |
| TECH × RD | Interaction term  |


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
