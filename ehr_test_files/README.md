# EHR JSON Test Files

These files are standalone upload fixtures for the application's "Upload EHR JSON" button. They are not wired into the app; select any file manually from this folder to exercise the recommendation engine and data-quality display.

Dates are chosen relative to the current project date, May 2, 2026.

## Cases

- `01_complete_urgent_t2dm_cardiorenal.json`
  - Complete T2DM profile with A1C above 10%, marked glucose elevation, symptoms, ASCVD, HFpEF, CKD, albuminuria, obesity, and MASH.
  - Expected signals: complete data, urgent/severe hyperglycemia branch, basal insulin priority, cardiorenal and weight/metabolic rationale.

- `02_mild_uncomplicated_t2dm.json`
  - Lower-risk T2DM profile above A1C target without ASCVD, HF, CKD, albuminuria, obesity, hypoglycemia risk, or cost barrier.
  - Expected signals: complete data, basic glycemia-above-target branch, metformin-style first-line fallback.

- `03_hf_ckd_without_severe_hyperglycemia.json`
  - T2DM with HFrEF, symptomatic HF, CKD stage 3-range eGFR, and albuminuria, but no severe hyperglycemia.
  - Expected signals: SGLT2 inhibitor prioritization for HF/CKD and cardiorenal rationale.

- `04_advanced_ckd_albuminuria.json`
  - T2DM with eGFR below 30 and albuminuria.
  - Expected signals: advanced CKD, reduced SGLT2 glycemic efficacy, GLP-1 RA priority in the simplified rule set.

- `05_weight_mash_priority.json`
  - T2DM with severe obesity, explicit weight-loss goal, MASH, and high liver fibrosis risk.
  - Expected signals: weight/metabolic and liver rationale; incretin-based classes prioritized.

- `06_oral_cost_hypoglycemia_constraints.json`
  - T2DM with oral-only preference, high cost sensitivity, and high hypoglycemia risk.
  - Expected signals: oral and lower-cost alternatives surface while sulfonylurea should not be promoted because hypoglycemia avoidance is prioritized.

- `07_missing_critical_values.json`
  - Missing diabetes diagnosis, age, sex, A1C, random glucose, eGFR, and albuminuria status.
  - Expected signals: insufficient data and missing critical-input warnings.

- `08_outdated_labs_partial_data.json`
  - Valid T2DM data with stale A1C, random glucose, eGFR, and UACR dates.
  - Expected signals: partial data confidence and outdated lab warnings while recommendations still render.

- `09_invalid_and_conflicting_values.json`
  - Invalid numeric strings, invalid dates, and HF listed without type.
  - Expected signals: missing critical values, parse/date conflicts, and HF type conflict.

- `10_mixed_t2dm_gestational_scope.json`
  - T2DM plus gestational diabetes.
  - Expected signals: T2DM recommendations plus scope-development notice for gestational diabetes.

- `11_type1_only_scope_redirect.json`
  - Type 1 diabetes only.
  - Expected signals: current-functionality-limited redirect rather than T2DM recommendations.

- `12_contraindications_and_current_therapy.json`
  - Severe T2DM profile where several classes are already in use and several classes are contraindicated.
  - Expected signals: existing therapies should not be re-recommended, contraindicated classes should move to Avoid, and severe glycemia logic should still be visible.

- `13_out_of_range_extreme_values.json`
  - Deliberately extreme numeric values with recent dates.
  - Expected signals: useful for seeing what the app clamps, accepts, or fails to quality-flag.

## Notes

The current importer reads `labs.random_glucose` for validation and internal random-glucose flags. The UI also displays `labs.fasting_glucose` and `labs.postprandial_glucose` when present, so most fixtures include all three glucose-style lab fields for future compatibility.
