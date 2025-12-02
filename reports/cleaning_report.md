# Cleaning & Aggregation Report

## Executive Summary
This document summarizes the data cleaning and aggregation steps performed on the healthcare insurance fraud detection dataset. The goal of this stage was to produce cleaned beneficiary, inpatient, and outpatient feature tables (for both Train and Test) without label leakage, ready for provider-level analysis and downstream modeling.

## Data Sources
- Raw data folder: `../data/raw/`
  - `Train_Beneficiarydata-1542865627584.csv`
  - `Test_Beneficiarydata-1542969243754.csv`
  - `Train_Inpatientdata-1542865627584.csv`
  - `Test_Inpatientdata-1542969243754.csv`
  - `Train_Outpatientdata-1542865627584.csv`
  - `Test_Outpatientdata-1542969243754.csv`
  - `Train-1542865627584.csv` (labels)
  - `Test-1542969243754.csv` (labels)

## Key Cleaning Steps
1. Beneficiary data
- Converted `DOB` and `DOD` to datetime, created `Age` (years) and `IsDeceased` (binary).
- Replaced binary-coded fields where needed (e.g., values of `2` -> `0`) to make columns binary.
- Converted `RenalDiseaseIndicator` from 'Y' to `1`.
- Dropped raw date columns after feature creation.
- Saved cleaned beneficiary files to `../data/processed/Train_Beneficiarydata_cleaned.csv` and `../data/processed/Test_Beneficiarydata_cleaned.csv`.

2. Inpatient claims
- Converted claim/admission/discharge date columns to datetime.
- Created temporal features: `ClaimDuration`, `AdmissionDuration`, `ClaimMonth`, `ClaimYear`, `ClaimDayOfWeek`.
- Aggregated at the provider level computing counts, unique beneficiaries, reimbursement stats (mean, sum, max, std, median), deductible stats, claim/admission duration stats, and unique physician counts.
- Filled `std` columns with `0` where `NaN` occurred (providers with a single claim).
- Derived additional features: claims per beneficiary, reimbursement per day, reimbursement per beneficiary, and total physicians.
- Computed diagnosis/procedure features: total unique diagnosis codes and procedure codes per provider.
- Saved outputs: `../data/processed/provider_inpatient_features.csv` and `../data/processed/test_provider_inpatient_features.csv`.

3. Outpatient claims
- Converted outpatient date columns and computed `ClaimDuration` and temporal fields.
- Implemented a reusable `process_outpatient` function that: converts dates, aggregates provider-level metrics (counts, reimbursement stats, deductibles, durations, physician counts), fills `std` NaNs with `0`, and derives claims-per-beneficiary and reimbursement-per-day metrics.
- Added outpatient diagnosis and procedure unique-code counts per provider.
- Saved outputs: `../data/processed/provider_outpatient_features.csv` and `../data/processed/test_provider_outpatient_features.csv`.

## Data Quality Notes
- Missing values: `std` statistics can be `NaN` for providers with a single claim — these were set to `0` to avoid propagation of NaNs into downstream aggregates.
- Date parsing used `errors='coerce'` to convert invalid dates to `NaT` and avoid crashes; derived durations use `NaN` when dates are missing.
- Labels were not merged into the final saved feature CSV files to prevent label leakage; labels are only merged temporarily during exploratory analysis and visualizations.

## Bug Fix / Important Fixes
- Fixed a data-loading typo where `test_outpatient_df` was misspelled as `est_outpatient_df`. That caused the test outpatient dataset to be skipped and led to identical Train/Test outpatient outputs. The variable name was corrected so the Test outpatient data is properly processed and saved separately.

## Saved Files (Processed)
- `../data/processed/Train_Beneficiarydata_cleaned.csv`
- `../data/processed/Test_Beneficiarydata_cleaned.csv`
- `../data/processed/provider_inpatient_features.csv`
- `../data/processed/test_provider_inpatient_features.csv`
- `../data/processed/provider_outpatient_features.csv`
- `../data/processed/test_provider_outpatient_features.csv`

## Results Summary
- Provider-level feature tables were created for both Train and Test splits and saved to `../data/processed/`.
- The cleaning step ensures features are ready for exploratory analysis, visualization (boxplots, heatmaps), and model training, without label contamination.

## Recommendations / Next Steps
- Verify row counts and provider counts in the processed CSVs by re-running the loading cell and printing `.shape` and `.nunique()` for `Provider`.
- Normalize or log-transform skewed monetary features (reimbursement sums) before modeling.
- Handle rare categorical codes (diagnosis/procedure) via grouping or embedding strategies.
- Address class imbalance when training (sampling, class weights, or specialized metrics).
- Save a manifest (small JSON) recording file hashes/timestamps for reproducibility.

---
Report saved to `reports/cleaning_report.md`.
