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
screening framework on real, open, aggregate business-dynamics data. The
demonstration is an **illustration of the screening procedure**, not a test of
predictive efficacy: it shows that the opportunity layer can separate weak
activity signals that lead the confirmed **activity-based indicators** (closure
and exit rates) from those that do not, and that the screening scorecard
discriminates between candidate proxies.

Terminology note: closure and exit rates are treated as **segment-level proxies
for credit deterioration, not direct measures of firm-level credit events**
(default, insolvency, arrears). A business may close or exit for reasons
unrelated to financial distress. No result here should be read as establishing
that a proxy predicts an individual firm's credit outcomes.

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
   ratio) and confirmed activity-based indicators (closure rate, exit rate).
2. Test stationarity with the Augmented Dickey–Fuller test.
3. Compute the cross-correlation function on differenced series over lags
   −12…+12 months (positive lag = signal leads). Two summaries are reported,
   and they are **deliberately different**:
   - `table2_ccf_peaks` records the peak of the **full** cross-correlation,
     which may fall at a negative or zero lag (contemporaneous or lagging);
   - the scorecard (`table4`) scores each proxy only on its best **leading**
     lag (lag > 0), so a strong *contemporaneous* association is **not**
     credited as anticipatory. This is why, e.g., the net-entry rate peaks
     contemporaneously against the closure rate yet receives low leadingness.
4. Test predictive value with Granger causality (lags 1–6; the 1–6 window is
   set a priori on operational grounds, not to maximise significance).
5. Combine leadingness, signal strength, predictive value, and freshness
   **compensatorily** into the opportunity-layer scorecard and screening
   verdict. Because the composite is compensatory, a proxy can reach ADOPT on
   strength of freshness/leading lag even if it fails the Granger test; the
   threshold (step 6) governs how much such compensation is allowed.
6. Vary the ADOPT threshold to show the scorecard discriminates: candidates
   fall away in ascending order of composite score (sensitivity analysis).
7. Repeat the leading relationship across provinces (robustness).

Note: a fifth opportunity criterion in the framework — **coverage / digital
divide bias** — is a qualitative *data-level precondition* assessed from a
source's provenance, not a time-series quantity, so it is not computed in this
notebook. The bundled whole-population administrative series satisfies it
trivially; see the manuscript (§5, Appendix A) for how it enters the judgement
record.

## Reproducing the results

Requires Python 3.10+.

```bash
pip install -r requirements.txt
cd notebooks        # the notebook resolves paths relative to notebooks/
jupyter nbconvert --to notebook --execute --inplace 01_leading_indicator_analysis.ipynb
```

The notebook writes to `../results` and `../data`, so it must be run **from the
`notebooks/` directory** (as above). All figures (PNG + PDF, 600 dpi,
greyscale) and tables (CSV) are written to `results/`. The pre-executed
`notebooks/01_leading_indicator_analysis.html` lets reviewers inspect the
outputs without running anything.

**Reproducibility.** The six result tables have been verified to regenerate
byte-for-byte, both on the version bounds in `requirements.txt` and on newer
releases (pandas 3.0, numpy 2.5, scipy 1.18, statsmodels 0.15); the results are
not version-sensitive.

## Outputs

| Output | Description |
|---|---|
| `fig1_series_overview` | Weak signals vs confirmed activity-based indicators over time |
| `fig2_ccf_leadlag` | Cross-correlation (lead–lag) panels |
| `fig3_scorecard_heatmap` | Opportunity-layer scorecard |
| `fig4_provincial_robustness` | Peak leading correlation across provinces |
| `fig5_threshold_sensitivity` | Screening verdicts as the ADOPT threshold varies |
| `table1`–`table6` | ADF, CCF peaks, Granger, scorecard, robustness, sensitivity |

## Scope and caveats

This package reproduces a **proof of concept of the screening procedure**,
not a deployed predictive model, and not a test of firm-level predictive
efficacy. The leading relationships are established at the segment (aggregate)
level between activity-based indicators that **proxy for** credit
deterioration; inference to individual firms is not implied, and the series
does not identify firms by size (it is SME-dominated by construction but SMEs
are not separable within it). The governance layer of the framework is
evaluated in the manuscript through a scenario walkthrough rather than in this
code. In the terms of the manuscript, this is an **early-stage design artifact
whose demonstration illustrates the opportunity-screening procedure**;
naturalistic validation with domain experts is specified as future work.

## License

Code is released under the MIT License (see `LICENSE`). The bundled data
remain subject to the Statistics Canada Open Licence; see `data/SOURCE.md`.
