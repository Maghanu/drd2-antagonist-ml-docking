# DRD2 Ligand Discovery

A reproducible project scaffold for discovering and prioritizing dopamine D2
receptor (DRD2) ligands using curated bioactivity data, machine learning, and
molecular docking.

> **Project status:** This repository is a scaffold. No assay exports,
prepared receptor structures, trained models, or implemented analysis scripts
are included yet. The numbered files in `src/` describe the intended pipeline
and must be implemented and validated before use.

## Repository layout

```text
.
├── data/
│   ├── raw/              # Untouched source exports; see data/README.md
│   └── processed/        # Curated activities and derived fingerprints
├── src/                  # Numbered analysis pipeline and shared utilities
├── notebooks/            # Numbered exploratory analyses only
├── docking/
│   ├── receptor/         # Prepared receptor structures
│   ├── ligands/          # Docking input molecules
│   └── runs/             # Per-ligand outputs and docking_summary.csv
├── models/               # Serialized trained models
├── results/
│   ├── figures/
│   └── tables/
└── docs/                 # Protocol and project notes
```

## Intended workflow

Implement and run the numbered scripts in order:

1. `src/01_curate_data.py` — curate and document DRD2 bioactivity records.
2. `src/02_fingerprints.py` — generate molecular representations.
3. `src/03_train_classification.py` — train and evaluate activity classifiers.
4. `src/04_train_regression.py` — train and evaluate potency regressors.
5. `src/05_scaffold_analysis.py` — analyze chemical scaffold coverage.
6. `src/06_screen_candidates.py` — prioritize candidates for follow-up.

Keep exploratory work in numbered notebooks under `notebooks/`. Record exact
commands, input provenance, software versions, and random seeds in the relevant
protocol notes under `docs/`. Do not overwrite raw exports; place transformed
data in `data/processed/`.

The source files are currently outlines, not runnable pipeline implementations.
Document input and output formats and validation criteria before implementing
each stage.

## Setup

Use a supported Python environment, then install the Python dependencies:

```bash
python -m venv .venv
```

Activate the environment (on Windows, use `.venv\Scripts\Activate.ps1`) and
install dependencies:

```bash
python -m pip install -r requirements.txt
```

Molecular docking is an optional, separate workflow. Follow
[docking/README.md](docking/README.md) for the WSL and AutoDock Vina setup; the
Python requirements do not install Vina or prepare receptor structures.

## Data and reproducibility

See [data/README.md](data/README.md) before adding any source export. Record the
source, retrieval date, target and assay identifiers, and every inclusion or
exclusion filter. Keep raw files unchanged and retain enough provenance to
recreate each processed table.

## Citation and license

Citation metadata is provided in [CITATION.cff](CITATION.cff). Code is made
available under the MIT License; see [LICENSE](LICENSE). Check the terms of any
external dataset and structure before redistribution.
