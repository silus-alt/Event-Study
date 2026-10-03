# Share Swap Alliance Event Study

This repository contains the empirical analysis conducted for:

> Yeh et al. (forthcoming). Market Reactions to Share Swap-Based Strategic Alliances: Evidence from Taiwanese Listed Companies. *Journal of Business Administration*.

The study examines cumulative abnormal returns (CAR) surrounding share swap alliance announcements in Taiwan using event study methodology.

## Key Findings

- No significant abnormal returns around the announcement; the market reaction appears delayed rather than immediate
- Leading firms earn higher CAR than their partnering firms over (-10, +10), after controlling for alliance and firm characteristics
- The value of technological alliances is weaker for firms with higher R&D intensity

## Research Design

- **Sample**: 8 share swap alliance events (15 firms) listed on TSE or TPEx, 2016–2025
- **Estimation Window**: [-240, -41]
- **Event Windows**: (-1, +1), (-5, +5), (-10, +10)
- **Model**: Market Model (OLS)
- **Data Source**: Taiwan Economic Journal (TEJ)

## Results Summary

### Full-Sample Test

Inference is based on the standardized cross-sectional test of mean CSAR; mean CAR is reported for economic magnitude (N = 15).

| Event Window | Mean CAR (%) | Mean CSAR | t (CSAR) | p (CSAR) |
|---|---:|---:|---:|---:|
| (-1, +1) | 2.13 | 0.87 | 1.46 | 0.144 |
| (-5, +5) | -0.93 | -0.05 | -0.03 | 0.973 |
| (-10, +10) | -1.32 | -0.31 | -0.20 | 0.838 |

Mean CSAR is not significantly different from zero in any event window.

### Cross-Sectional Regression

OLS regression of CAR on alliance and firm characteristics (N = 15). Significant variables:

| Variable | Direction | Significant in |
|---|:---:|---|
| TECH | + | (-5, +5), (-10, +10) |
| LEAD | + | (-10, +10) |
| BM | − | (-10, +10) |
| TECH × RD | − | (-10, +10) |

Significance at the 5% level or better. No variable is significant in the (-1, +1) window, and explanatory power rises with window length, consistent with a delayed market reaction. Full results are in `06_regression.ipynb`.

## Robustness Checks

### 1. Additional Event Days and Windows

To check whether the null result depends on the choice of symmetric windows, mean CSAR is re-tested on the announcement day, the following day, and three post-announcement windows.

| Period | Mean CSAR | t (CSAR) |
|---|---:|---:|
| (0) | -0.02 | -0.08 |
| (+1) | 0.45 | 0.62 |
| (0, +1) | 0.43 | 0.61 |
| (0, +5) | -0.65 | -0.49 |
| (0, +10) | -0.79 | -0.55 |

None of the periods shows a significant abnormal return, confirming the absence of an immediate market reaction.

### 2. Control Variables

The cross-sectional regression is re-estimated with firm size (SIZE, log of total assets) and financial leverage (LEV, total debt to total assets), each from the fiscal year before the announcement. Results for the (-10, +10) window:

| Variable | Baseline | + SIZE | + LEV |
|---|:---:|:---:|:---:|
| TECH | + \*\* | + \*\* | + \*\*\* |
| LEAD | + \*\* | + \* | + \*\* |
| BM | − \*\*\* | − \*\*\* | − \*\*\* |
| TECH × RD | − \*\* | − \* | − \*\*\* |
| Control | — | n.s. | + \*\* |

\*\*\* p < 0.01, \*\* p < 0.05, \* p < 0.1; n.s. = not significant.

All four variables keep their sign and remain significant under both specifications. SIZE has no effect, while LEV is positively associated with CAR. The main conclusions therefore do not stem from omitted firm size or leverage effects.

## Data Availability

Raw data were obtained from the Taiwan Economic Journal (TEJ) database and **are not included in this repository** due to licensing restrictions. All notebook outputs are preserved, so results can be reviewed without the data. See [`data/README.md`](data/README.md) for the required file structure to reproduce the analysis with your own TEJ access.

## Repository Structure

```
event-study/
├── data/
│   └── README.md                     # Required data files and structure
└── notebooks/
    ├── 01_ols_demo.ipynb
    ├── 02_data_cleaning.ipynb
    ├── 03_line_chart.ipynb
    ├── 04_all_samples_test.ipynb
    ├── 05_subgroup_test.ipynb
    ├── 06_regression.ipynb
    ├── 07_robustness_windows.ipynb
    └── 08_robustness_controls.ipynb
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
| TECH × RD | Interaction term (RD mean-centered) |

**7. Robustness: Additional Windows** (`07_robustness_windows.ipynb`)

Re-tests mean CAR and CSAR on event days (0) and (+1), and windows (0, +1), (0, +5), (0, +10), using the same cross-sectional tests as `04`.

**8. Robustness: Control Variables** (`08_robustness_controls.ipynb`)

Re-estimates the regression in `06`, adding SIZE or LEV as a control variable.

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
