# IT2011 - Artificial Intelligence and Machine Learning
## Group Assignment — Progress Review I: Data Preprocessing and EDA

**Group ID:** 2026-Y2-S1-KU-56
**Assigned Dataset:** Chronic Kidney Disease (CKD) Dataset — UCI Machine Learning Repository
Source: https://archive.ics.uci.edu/dataset/336/chronic+kidney+disease

## Group Members

| IT Number | Name | Assigned Preprocessing Technique |
|---|---|---|
| IT25103993 | Hansaka K. L. G. S. | Handling Missing Data |
| IT25103994 | Amarathunge W. R. P. N. | Encoding Categorical Variables |
| IT25103995 | Samarakoon S. M. D. S. | Outlier Detection & Treatment |
| IT25103996 | Gunasekara K. J. W. | Normalization / Scaling |
| IT25103997 | Senavirathna K. V. L. C. M. | Feature Engineering — Feature Selection |
| IT25103998 | Dissanayake D. M. A. S. K. | Feature Engineering — Dimensionality Reduction (PCA) |

## Problem Domain

**Healthcare** — early detection of Chronic Kidney Disease (CKD) from routine clinical and laboratory
measurements (blood pressure, blood glucose, hemoglobin, specific gravity, etc.). The dataset contains 400
patient records (250 CKD, 150 not-CKD) with 24 features (11 numeric, 13 nominal) plus the binary class
label, collected by Apollo Hospitals, Tamil Nadu, India.

## Repository Structure

```
Group_ID/
├── README.md                         — this file
├── data/
│   ├── raw/                          — dataset as provided (ARFF + info file + a parsed CSV copy)
│   └── external/                     — (none used)
├── notebooks/
│   ├── IT25103993_Hansaka_Missing_Data_Handling.ipynb
│   ├── IT25103994_Amarathunge_Categorical_Encoding.ipynb
│   ├── IT25103995_Samarakoon_Outlier_Treatment.ipynb
│   ├── IT25103996_Gunasekara_Feature_Scaling.ipynb
│   ├── IT25103997_Senavirathna_Feature_Selection.ipynb
│   ├── IT25103998_Dissanayake_PCA_Dimensionality_Reduction.ipynb
│   └── group_pipeline.ipynb          — integrated pipeline (combined work)
└── results/
    ├── eda_visualizations/           — all plots (PNG), named by IT number
    ├── logs/                         — ARFF parsing log (1 malformed record dropped, see below)
    └── outputs/                      — each member's processed CSV + final group pipeline outputs
```

## Data Note (important — mention in the viva)

The raw `.arff` file has a known data-quality quirk: **one record (row 370 of 400)** has a malformed field
count (an extra stray comma shifts every value after `dm` by one column, corrupting `appet`, `pe`, `ane`).
This single corrupted record was dropped during the initial ARFF→CSV parsing step, documented in
`results/logs/arff_parsing_log.txt`. The working dataset therefore has **399 records** (250 CKD / 149
not-CKD) from this point onward. All missing values (`?` in the original file) were preserved as `NaN` in
`data/raw/ckd_clean_raw.csv` — this is the "raw, as provided" file every member's notebook starts from.

## How to Run

1. Open `notebooks/` in Jupyter/Google Colab.
2. Run each `IT_Number_...ipynb` notebook independently (each reads the previous member's output from
   `results/outputs/` and writes its own) — this mirrors the assignment's required pipeline order:
   Missing Data → Encoding → Outliers → Scaling → Feature Selection / PCA.
3. Run `group_pipeline.ipynb` to see the entire pipeline reproduced end-to-end in one notebook, from the raw
   data to the two final model-ready datasets:
   - `results/outputs/group_pipeline_feature_selected.csv` (12 mutual-information-selected features)
   - `results/outputs/group_pipeline_pca_reduced.csv` (PCA components explaining 90% variance)

## Individual Preprocessing Summaries

- **Missing Data (Hansaka):** Median imputation for 11 numeric features, mode imputation for 13 categorical
  features — chosen over row/column deletion because several features are missing in up to ~38% of records.
- **Encoding (Amarathunge):** Label encoding for 11 binary nominal features + direct numeric conversion for
  3 ordinal-coded features (`sg`, `al`, `su`).
- **Outlier Treatment (Samarakoon):** IQR-based detection, treated via capping/winsorizing (not deletion) to
  avoid removing genuine severe-CKD cases from an already small (399-row) dataset.
- **Scaling (Gunasekara):** Compared StandardScaler vs MinMaxScaler; StandardScaler selected as the pipeline
  default due to residual skew in lab values even after outlier capping.
- **Feature Selection (Senavirathna):** Mutual-information ranking; top 12 features retained.
- **PCA (Dissanayake):** Dimensionality reduced from 24 → components explaining 90% of variance; 2D PCA
  projection shows visibly separable CKD / not-CKD clusters.

## AI Tool Usage Declaration

Claude (Anthropic) was used to help build the preprocessing pipeline, generate the EDA code/visualizations,
and draft this README, as permitted by the assignment's AI Tool Usage Declaration policy. All code was
executed and its outputs verified by the group before inclusion. **Each member should be ready to personally
explain the logic, justification, and output of their own assigned technique in the viva** — this is
individually assessed and cannot be substituted by reading from the notebook.
