# DRD2 pIC50 prediction model

## Purpose

This directory stores the fitted regression model used to predict pIC50 for
new compounds. The model supports ranking and prioritization for follow-up; it
does not establish that a compound binds DRD2 or acts as an antagonist.

## Current artifact

- `final_svr_morgan2.joblib` is the selected support-vector regression (SVR)
  model trained with Morgan radius-2 fingerprints.
- The model was selected by the evaluation workflow in
  [`src/04_ml.ipynb`](../src/04_ml.ipynb), which compares random-forest, SVR,
  and ANN regressors across MACCS, Morgan-radius-2, and Morgan-radius-3
  representations.
- [`src/05_pIC50prediction.ipynb`](../src/05_pIC50prediction.ipynb) loads the
  model to predict pIC50 for scaffold-similarity and random ZINC22 collections.

The model expects the same fingerprint representation used during training.
Use the notebook's prediction helper rather than passing raw SMILES directly
to the serialized estimator.

## Reproducibility and limitations

Evaluation figures and metric tables are saved in `results/`. When replacing
or retraining the artifact, record the training-data version, activity
definition, fingerprint settings, train/test split and cross-validation
strategy, random seed, software versions, and evaluation metrics. pIC50
predictions are estimates whose reliability depends on the training data and
the chemical similarity of the query compounds to that data; they are not
experimental measurements.
