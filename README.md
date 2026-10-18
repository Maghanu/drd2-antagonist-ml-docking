# DRD2 Ligand Potency Prediction and Docking

## Project purpose

This research project builds a computational workflow to identify and
prioritize compounds that may have activity at the human dopamine D2 receptor
(DRD2). It combines curated DRD2 pIC50 measurements, molecular fingerprints,
machine-learning regression, chemical clustering and scaffold analysis, and
AutoDock Vina docking.

The goal is **computational prioritization for further investigation**, not
proof that a compound binds DRD2 or acts as an antagonist. pIC50 values describe
assay potency and do not, by themselves, establish a compound's mechanism.
Likewise, Vina scores are approximate docking scores, not experimental
affinities. Predictions and scores should be treated as hypotheses for
follow-up, not as experimental findings.

## Workflow

The numbered notebooks in `src/` form the main analysis:

1. [`01_ic50.ipynb`](src/01_ic50.ipynb) retrieves DRD2 bioactivity records from
   ChEMBL, standardizes eligible IC50 measurements to pIC50, and writes the
   curated compound table.
2. [`02_clustering.ipynb`](src/02_clustering.ipynb) groups compounds by
   molecular-fingerprint similarity and saves cluster summaries and figures.
   It writes the selected cluster-6 molecules to an SDF for follow-on analysis.
3. [`03_scaffold.ipynb`](src/03_scaffold.ipynb) analyzes Bemis-Murcko scaffolds
   in that selected cluster.
4. [`04_ml.ipynb`](src/04_ml.ipynb) generates MACCS and Morgan fingerprints,
   benchmarks regression models for pIC50, evaluates them using a single split
   and cross-validation, and saves the selected model.
5. [`05_pIC50prediction.ipynb`](src/05_pIC50prediction.ipynb) applies the saved
   Morgan-radius-2 model to two ZINC22 sets: 1,648 scaffold-similarity
   compounds and 10,000 random compounds.
6. [`06_docking.ipynb`](src/06_docking.ipynb) docks the top 100 predicted
   compounds from each ZINC22 set to DRD2 and compares model and docking
   rankings.
7. [`07_unwanted-substructures.ipynb`](src/07_unwanted-substructures.ipynb)
   screens **only the scaffold set's successfully docked top-100 shortlist**
   for PAINS and unwanted substructures using RDKit catalogs. It does not
   screen the full scaffold set or the random set.

The intended data flow is:

```text
ChEMBL DRD2 IC50 data
  -> curated pIC50 compounds
  -> fingerprint/model evaluation
  -> predictions for scaffold and random ZINC22 compounds
  -> top-100 docking per set
  -> PAINS/BRENK filtering of the scaffold top-100 shortlist
```

## Repository layout

```text
data/
  raw/              Source activity and ZINC22 compound tables
  processed/        Molecular fingerprint tables
src/                Numbered analysis notebooks and shared utilities
Workflow/           Earlier workflow notebook
models/             Saved regression model
docking/            Receptor preparation, ligands, poses, logs, and summaries
results/
  figures/          Cluster, model-evaluation, and docking-comparison figures
  tables/           Cluster summaries, predictions, docking scores, and metrics
docs/               Additional project notes
```

## Running the notebooks

Run the notebooks in order where their outputs are inputs to later stages.
Open the repository folder in VS Code, select a Python kernel with the
notebook's dependencies, and run the cells from top to bottom. The docking
notebook requires AutoDock Vina and Meeko; on Windows, use a Linux kernel in
WSL 2. See [`docking/README.md`](docking/README.md) for its environment and
structure-preparation details.

Outputs are written under `data/processed/`, `models/`, `docking/`, and
`results/`. Do not overwrite raw inputs when regenerating derived data.

## Interpretation and reproducibility

Record the data source, query, retrieval date, filters, software versions, and
model settings when regenerating results. Keep raw data distinct from processed
data. Review docked poses and validate the approach against appropriate
reference compounds before relying on docking rankings. See
[`data/README.md`](data/README.md) for data provenance and
[`models/README.md`](models/README.md) for the saved model.

## Citation and license

Citation metadata is provided in [`CITATION.cff`](CITATION.cff). Code is made
available under the MIT License; see [`LICENSE`](LICENSE). Check the terms of
any external dataset and structure before redistribution.
