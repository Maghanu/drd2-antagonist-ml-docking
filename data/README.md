# Project data and provenance

## What the data are used for

This project uses DRD2 IC50 measurements to train and evaluate a model that
predicts pIC50, then applies that model to two ZINC22 compound collections.
These predictions support computational candidate prioritization; they do not
establish that a compound is a DRD2 antagonist. The project also uses a DRD2
crystal structure for docking. See the repository
[README](../README.md) for the complete workflow.

## Directory contents

- `raw/drd2_curated_compounds.csv` is the 435-compound DRD2 activity table used
  for fingerprint generation and model training. It contains compound IDs,
  pIC50 values, and SMILES.
- `raw/scaffold_zinc22.csv` contains 1,648 ZINC22 compounds retrieved by
  scaffold/similarity search.
- `raw/random_zinc22.csv` contains 10,000 ZINC22 compounds from a random
  search.
- `processed/DopamineD2_maccs.csv`,
  `processed/DopamineD2_morgan2.csv`, and
  `processed/DopamineD2_morgan3.csv` contain MACCS, Morgan-radius-2, and
  Morgan-radius-3 fingerprints generated from the DRD2 activity table.
- `raw/crystal_structures/` stores the unmodified 6CM4 structure used as the
  source for the docking-box reference ligand.
- `processed/crystal_structures/` stores the receptor structure prepared for
  the DRD2 docking workflow.

The analysis notebooks in `src/` read these files and write predictions,
metrics, cluster analyses, and docking results under `results/` and `docking/`.
Do not edit raw input files in place when regenerating analyses.

## Provenance and scientific interpretation

The current source queries and missing provenance fields are listed in
[`docs/reproducibility.md`](../docs/reproducibility.md). In particular, the
checked-in ChEMBL activity table does not record the ChEMBL release or access
date, and the ZINC22 files do not record their release/retrieval dates or
complete queries. Their first Git commit dates are not substitutes for
retrieval dates.

When retrieving or replacing a source dataset, record its source URL or query,
retrieval date, version, and checksum. Document the filtering and
standardization steps used to produce the curated pIC50 data, including assay
selection, units, duplicate handling, and excluded-record counts. Preserve
original activity measurements and units when possible.

IC50 is an assay measurement, not a mechanism label. Do not infer antagonism
from potency alone; retain assay and mechanism information wherever available.
ZINC22 predictions are model estimates, and docking scores are approximate
computational scores. Neither substitutes for experimental validation.
The ZINC22 scaffold set was selected by similarity search; do not describe
that alone as demonstrated scaffold hopping.
