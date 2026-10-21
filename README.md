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

1. [`Group6_codeACMDD_step01_ic50.ipynb`](src/Group6_codeACMDD_step01_ic50.ipynb) retrieves DRD2 bioactivity records from
   ChEMBL, standardizes eligible IC50 measurements to pIC50, and writes the
   curated compound table.
2. [`Group6_codeACMDD_step02_clustering.ipynb`](src/Group6_codeACMDD_step02_clustering.ipynb) groups compounds by
   molecular-fingerprint similarity and saves cluster summaries and figures.
   It writes the selected cluster-6 molecules to an SDF for follow-on analysis.
3. [`Group6_codeACMDD_step03_scaffold.ipynb`](src/Group6_codeACMDD_step03_scaffold.ipynb) analyzes Bemis-Murcko scaffolds
   in that selected cluster.
4. [`Group6_codeACMDD_step04_ml.ipynb`](src/Group6_codeACMDD_step04_ml.ipynb) generates MACCS and Morgan fingerprints,
   benchmarks regression models for pIC50, evaluates them using a single split
   and cross-validation, and saves the selected model.
5. [`Group6_codeACMDD_step05.ipynb`](src/Group6_codeACMDD_step05.ipynb) applies the saved
   Morgan-radius-2 model to two ZINC22 sets: 1,648 scaffold-similarity
   compounds and 10,000 random compounds.
6. [`Group6_codeACMDD_step06_docking_6CM4.ipynb`](src/Group6_codeACMDD_step06_docking_6CM4.ipynb) docks the top 100 predicted
   compounds from each ZINC22 set to DRD2 and compares model and docking
   rankings.
7. [`Group6_codeACMDD_step07_unwanted-substructures.ipynb`](src/Group6_codeACMDD_step07_unwanted-substructures.ipynb)
   screens **only the scaffold set's successfully docked top-100 shortlist**
   for PAINS and unwanted substructures using RDKit catalogs. It does not
   screen the full scaffold set or the random set.

Additional docking analyses are available separately from the main sequence:

- [`Group6_codeACMDD_step06_docking-comparisons.ipynb`](src/Group6_codeACMDD_step06_docking-comparisons.ipynb)
  docks named reference compounds against DRD2 structure 6CM4_Na.
- [`Group6_codeACMDD_step06_active.ipynb`](src/Group6_codeACMDD_step06_active.ipynb)
  compares the five selected hits and risperidone in inactive 6CM4_Na and
  active 9BS9 DRD2 structures.
- [`Group6_codeACMDD_step08_docking_6BQh.ipynb`](src/Group6_codeACMDD_step08_docking_6BQh.ipynb)
  docks the selected hits and reference ligands against 5-HT2C structure 6BQH.

See [`docking/README.md`](docking/README.md) and
[`docs/reproducibility.md`](docs/reproducibility.md) for their inputs,
settings, outputs, and limitations.

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
src/                Numbered analysis notebooks
models/             Saved regression model
docking/            Receptor preparation, ligands, poses, logs, and summaries
results/
  presentation_figures/ Figures prepared for project presentations
  figures/          Cluster, model-evaluation, and docking-comparison figures
  tables/           Cluster summaries, predictions, docking scores, and metrics
docs/               Additional project notes
requirements.txt    Pinned dependencies for core analysis
```

## Running the notebooks

Run the notebooks in order where their outputs are inputs to later stages.
The pinned core-analysis dependencies are in the repository-root
[`requirements.txt`](requirements.txt). From the repository root, install
them with:

```bash
python -m pip install -r requirements.txt
```

Docking additionally requires the Conda packages documented in
[`docking/README.md`](docking/README.md). Notebook metadata records runs under
different Python versions; use the documented Python 3.12 environment for
new runs. On Windows, run docking with a Linux kernel in WSL 2.

Outputs are written under `data/processed/`, `models/`, `docking/`, and
`results/`. Do not overwrite raw inputs when regenerating derived data.

## Interpretation and reproducibility

The repository includes reference-compound docking, a paired active/inactive
DRD2 comparison, and a selected-ligand 5-HT2C docking workflow. These
computational workflows do not establish experimental affinity or antagonist
activity, and they do not reproduce molecular-dynamics analyses. The report
and its figures are not included, so their exact inputs and procedures cannot
be verified here. Known provenance gaps are listed in
[`docs/reproducibility.md`](docs/reproducibility.md). Keep raw data distinct
from processed data. Review docked poses and validate the approach against
appropriate reference compounds before relying on docking rankings. See
[`data/README.md`](data/README.md) for data provenance and
[`models/README.md`](models/README.md) for the saved model.

## Citation and license

Citation metadata is provided in [`CITATION.cff`](CITATION.cff). Code is made
available under the MIT License; see [`LICENSE`](LICENSE). Check the terms of
any external dataset and structure before redistribution.
