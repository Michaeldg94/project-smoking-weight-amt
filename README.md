# Smoking Quitting and Weight Gain: A Causal Analysis

This project estimates the causal effect of quitting smoking on weight gain over a 10-year period. It uses observational data from the National Health and Nutrition Examination Survey Epidemiologic Follow-up Study (NHEFS) and applies three semiparametric estimators to identify a robust average treatment effect.

All three methods find that quitting smoking causes a weight gain of roughly 3 kg.

## Key results

| Method                               | Estimate (kg) | 95% CI       |
|--------------------------------------|---------------|--------------|
| Partial linear regression (DoubleML) | 3.30          | [2.33, 4.26] |
| Inverse probability weighting        | 2.95          | [1.92, 3.90] |
| Double Lasso                         | 3.18          | [2.29, 4.07] |

These estimates align with the benchmark in Hernan and Robins (2020) of approximately 3.4 kg.

## Data

The NHEFS tracks 1,566 adults from a 1971 baseline to a 1982 follow-up. The treatment variable is binary: quit smoking or not. The outcome is weight change in kilograms. The dataset includes more than 60 baseline covariates covering demographics, smoking history, exercise and health status.

Source files sit in `data/`:

-   `nhefs_complete.csv` — full analytical sample
-   `nhefs_codebook.csv` — variable definitions

## Methods

1.  **Causal structure learning.** A PC algorithm with a kernel conditional independence test learns the adjustment set from data, guided by temporal background knowledge.
2.  **Partial linear regression.** DoubleML with XGBoost nuisance models residualizes both treatment and outcome before estimating the effect via OLS.
3.  **Inverse probability weighting.** XGBoost estimates propensity scores. Stabilized Hajek weights rebalance the sample.
4.  **Double Lasso.** Two penalized regressions select confounders for treatment and outcome separately. OLS estimates the effect on the residuals.

## Project structure

```         
.
├── final_report_justified.ipynb   # Main analysis notebook
├── final_report_justified.html    # Rendered HTML report
├── data/
│   ├── nhefs_complete.csv
│   └── nhefs_codebook.csv
├── output/
│   ├── figures/                   # 13 diagnostic and result plots
│   └── results/                   # Summary tables as CSV
├── pyproject.toml
└── uv.lock
```

## Getting started

Requires Python 3.13 or later.

``` bash
# Clone the repo
git clone <repo-url>
cd project-smoking-weight-amt

# Install dependencies with uv
uv sync

# Or with pip
pip install jupyter pandas numpy

# Additional packages used in the notebook
pip install doubleml xgboost scikit-learn statsmodels causal-learn seaborn matplotlib
```

Open and run `final_report_justified.ipynb` to reproduce the full analysis.

## Authors

-   Mircea-Adrian Guinea
-   Michael Duarte Gonçalves
-   Tina Sikharulidze

MSc in Data Science Methodology (2026), Barcelona School of Economics - *Causal Inference and Machine Learning*.