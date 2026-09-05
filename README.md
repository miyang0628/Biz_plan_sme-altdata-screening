# Governance-Embedded Opportunity Screening — Reproducibility Package

Anonymous code and data package accompanying the manuscript
*"Governance-Embedded Opportunity Screening: A Design Science Framework for
Planning Alternative-Data Credit Services in the SME Segment,"* currently
under double-blind review.

> **Anonymity notice.** This repository is released for anonymous peer review.
> It intentionally contains no author names, affiliations, institutional
> identifiers, funding information, or contact details. Please do not open
> issues or pull requests that could de-anonymise the authors during review.

---

## What this package reproduces

The manuscript's empirical demonstration and its threshold-sensitivity
evaluation. Both exercise the **opportunity layer** of the proposed
screening framework on real, open, aggregate business-dynamics data,
establishing that weak activity signals lead confirmed credit-event
indicators and that the screening scorecard discriminates between candidate
proxies.

The analysis uses only **aggregate** public data — no individual firm or
personal records — so it carries no privacy exposure while still exercising
the screening logic on genuine activity data.

## Repository structure

```
.
├── data/
│   ├── SOURCE.md                                   # full data provenance
│   ├── canada_business_dynamics_national_monthly.csv
│   └── canada_business_dynamics_provincial_monthly.csv
├── notebooks/
│   ├── 01_leading_indicator_analysis.ipynb         # main analysis (runnable)
│   └── 01_leading_indicator_analysis.html          # executed, rendered copy
└── results/
    ├── figures/                                    # PNG + PDF, 600 dpi, greyscale
    └── tables/                                     # CSV outputs
```

## Data source

Statistics Canada, Table 33-10-0270-01, *Experimental estimates for business
openings and closures*, monthly, seasonally adjusted, 2015-01 to 2026-05.
Retrieved via the Statistics Canada Web Data Service. Released under the
Statistics Canada Open Licence. Full details in [`data/SOURCE.md`](data/SOURCE.md).

Two cleaned panels are provided:
- national business-sector aggregate (wide by dynamics measure);
- ten provinces (long form), used for the robustness check.

## Method (as implemented in the notebook)

1. Construct rate series (per 1,000 active businesses) for candidate weak
   signals (opening rate, entrant rate, net-entry rate, opening-to-closure
   ratio) and confirmed indicators (closure rate, exit rate).
2. Test stationarity with the Augmented Dickey–Fuller test.
3. Establish leadingness with the cross-correlation function on differenced
   series (positive lag = signal leads).
4. Test predictive value with Granger causality (lags 1–6).
5. Combine the evidence into the opportunity-layer scorecard and screening
   verdict.
6. Vary the ADOPT threshold to show the scorecard discriminates
   (sensitivity analysis).
7. Repeat the leading relationship across provinces (robustness).

## Reproducing the results

Requires Python 3.10+.

```bash
pip install -r requirements.txt
cd notebooks
jupyter nbconvert --to notebook --execute --inplace 01_leading_indicator_analysis.ipynb
```

All figures (PNG + PDF, 600 dpi, greyscale) and tables (CSV) are written to
`results/`. The pre-executed `notebooks/01_leading_indicator_analysis.html`
lets reviewers inspect the outputs without running anything.

## Outputs

| Output | Description |
|---|---|
| `fig1_series_overview` | Weak signals vs confirmed indicators over time |
| `fig2_ccf_leadlag` | Cross-correlation (lead–lag) panels |
| `fig3_scorecard_heatmap` | Opportunity-layer scorecard |
| `fig4_provincial_robustness` | Peak leading correlation across provinces |
| `fig5_threshold_sensitivity` | Screening verdicts as the ADOPT threshold varies |
| `table1`–`table6` | ADF, CCF peaks, Granger, scorecard, robustness, sensitivity |

## Scope and caveats

This package reproduces a **proof of concept of the screening procedure**,
not a deployed predictive model. The leading relationships are established at
the segment (aggregate) level; inference to individual firms is not implied.
The governance layer of the framework is evaluated in the manuscript through
a scenario walkthrough rather than in this code.

## License

Code is released under the MIT License (see `LICENSE`). The bundled data
remain subject to the Statistics Canada Open Licence; see `data/SOURCE.md`.
