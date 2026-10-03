# Share Swap Alliance Event Study

This repository contains the empirical analysis conducted for:

> Yeh et al. (2026). *Market Reactions to Share Swap-Based Strategic Alliances: Evidence from Taiwanese Listed Companies*. National Taipei University.

The study examines cumulative abnormal returns (CAR) surrounding share swap alliance announcements in Taiwan using event study methodology.

## Key Findings

- No significant abnormal returns in the short term
- Market reaction appears delayed rather than immediate
- The value of technological alliances is weaker for firms with higher R&D intensity

## Research Design

- **Sample**: 8 share swap alliance events (15 firms) listed on TSE or TPEx, 2016–2025
- **Estimation Window**: [-240, -41]
- **Event Windows**: (-1, +1), (-5, +5), (-10, +10)
- **Model**: Market Model (OLS)
- **Data Source**: Taiwan Economic Journal (TEJ)


## Results Summary

Full-sample test (N = 15). Inference is based on the standardized cross-sectional test of mean CSAR; mean CAR is reported for economic magnitude.

| Event Window | Mean CAR (%) | Mean CSAR | t (CSAR) | p (CSAR) |
|---|---:|---:|---:|---:|
| (-1, +1) | 2.13 | 0.87 | 1.46 | 0.144 |
| (-5, +5) | -0.93 | -0.05 | -0.03 | 0.973 |
| (-10, +10) | -1.32 | -0.31 | -0.20 | 0.838 |

Mean CSAR is not significantly different from zero in any event window. Cross-sectional regressions on alliance and firm characteristics are in `06_regression.ipynb`.

## Data Availability

Raw data were obtained from the Taiwan Economic Journal (TEJ) database and **are not included in this repository** due to licensing restrictions. All notebook outputs are preserved, so results can be reviewed without the data. See [`data/README.md`](data/README.md) for the required file structure to reproduce the analysis with your own TEJ access.

## Repository Structure

```
event-study/
├── data/
│   └── README.md                 # Required data files and structure
└── notebooks/
    ├── 01_ols_demo.ipynb         
    ├── 02_data_cleaning.ipynb    
    ├── 03_line_chart.ipynb       
    ├── 04_all_samples_test.ipynb 
    ├── 05_subgroup_test.ipynb    
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

**5. Subgroup Tests** (`05_subgroup_test.ipynb`)

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

- AR and SAR for all firms were exported by TEJ. `01_ols_demo.ipynb` demonstrates the underlying market model calculation using Foxconn as an example.
