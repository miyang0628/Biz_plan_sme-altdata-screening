# Data Provenance

## Source
Statistics Canada. Table 33-10-0270-01 (formerly product 33100270).
*Experimental estimates for business openings and closures for Canada, provinces and territories,
census metropolitan areas, seasonally adjusted.*

- Frequency: Monthly
- Coverage: 2015-01 to 2026-05
- Dimensions: Geography (49), Industry (32), Business dynamics measure (8:
  Active, Opening, Continuing, Closing, Reopening, Entrants, Temporary closures, Exits)
- Licence: Statistics Canada Open Licence
- Retrieved via: StatCan Web Data Service (WDS) full-table CSV endpoint
  `https://www150.statcan.gc.ca/t1/wds/rest/getFullTableDownloadCSV/33100270/en`

## Derived files in this folder

### canada_business_dynamics_national_monthly.csv
National, business-sector-industries aggregate. Pivoted wide by dynamics measure.
Index = month (REF_DATE). 137 monthly observations.

### canada_business_dynamics_provincial_monthly.csv
Ten provinces, business-sector-industries aggregate, long form
(REF_DATE, GEO, Industry, Business dynamics measure, VALUE). Used for robustness.

## Notes
Only aggregate counts are used. No individual business or personal records are involved.
Rates in the analysis are normalised per 1,000 active businesses.
