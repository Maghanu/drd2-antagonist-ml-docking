# Reproducibility record and known gaps

This file distinguishes what the checked-in code records from what cannot be
recovered from the committed data and notebook metadata. A Git commit date
only shows when a file entered the repository; it is not a dataset retrieval
date.

## Bioactivity and candidate data

| Dataset | Recorded source and query | Release/retrieval provenance |
| --- | --- | --- |
| DRD2 activity table (`data/raw/drd2_curated_compounds.csv`, 435 compounds) | `src/Group6_codeACMDD_step01_ic50.ipynb` queries the ChEMBL REST API for UniProt `P14416`, selects target `CHEMBL217`, and requests `IC50`, exact relation `=`, binding assay type `B`; it then retains values in nM. | ChEMBL release and API access date were not saved in the notebook or CSV. The file was in Git by 2026-10-16; that is not its retrieval date. |
| Scaffold collection (`data/raw/scaffold_zinc22.csv`, 1,648 compounds) | ZINC22 similarity search; `src/Group6_codeACMDD_step05.ipynb` describes default similarity-search settings. | ZINC22 release/revision, full query parameters/reference, and retrieval date were not saved. The file was in Git by 2026-10-16; that is not its retrieval date. |
| Random collection (`data/raw/random_zinc22.csv`, 10,000 compounds) | ZINC22 random-search query; the exact query is not included in the repository. | ZINC22 release/revision and retrieval date were not saved. The file was in Git by 2026-10-16; that is not its retrieval date. |

The existing data files do not contain enough metadata to reconstruct the
missing release identifiers or retrieval dates. Do not infer them from file
timestamps or Git history. Record the query response metadata and access date
when the source data are next retrieved.

## Receptor used by the checked-in docking workflow

Notebook 06 reads the receptor from the committed local
`data/processed/crystal_structures/6CM4_Na.mmcif`; it performs no structure
download. It retains DRD2 `ATOM` records at author residue numbers 35–222 and
364–442, excluding the T4 lysozyme fusion and unrelated records. The Na+ ion
from this processed file is included in the final Vina receptor PDBQT. PDBFixer
and restrained OpenMM minimization are applied to protein atoms; the sodium
is added to the PDBQT after that preparation, rather than being minimized or
parameterized by the protein force field.

Because the processed receptor file omits risperidone, notebook 06 reads
`data/raw/crystal_structures/6cm4.mmcif` locally only to get the native
risperidone (component `8NU`) coordinates for the docking box. The box has 8 Å
padding in each dimension. Both files are committed inputs. The prepared
receptor cache is `docking/prepared/6CM4_Na/`.

Suggested Methods wording for analyses produced by notebook 06:

> The DRD2 receptor was prepared from the local processed 6CM4_Na mmCIF file.
> DRD2 `ATOM` records at author residue numbers 35–222 and 364–442 were
> retained, excluding the T4 lysozyme fusion. The structure's Na+ ion was
> included in the final receptor PDBQT after protein preparation. Native
> risperidone (component 8NU) coordinates from the local raw 6CM4 mmCIF were
> used only to define the docking box, with 8 Å padding. Missing protein atoms
> were rebuilt with PDBFixer, the protein was minimized with positional
> restraints in OpenMM, and Meeko generated the protein receptor PDBQT.

The report and its Figure 4 are not present in this repository, and
`docking/runs/docking_summary.csv` contains only its header. Thus the exact
receptor and procedures behind Figure 4 and the unimplemented control docking
and MD analyses cannot be verified here. Use the Methods wording above only
if Figure 4 was generated from the current notebook workflow. Existing docking
scores and figures should be regenerated before describing them as results
from this Na+-containing receptor.

## Software environment records

Notebook kernel metadata is not consistent across saved notebooks:

| Notebook(s) | Python version in saved metadata |
| --- | --- |
| `src/Group6_codeACMDD_step01_ic50.ipynb` | 3.14.7 |
| `src/Group6_codeACMDD_step02_clustering.ipynb`–`src/Group6_codeACMDD_step05.ipynb` | 3.12.2 |
| `src/Group6_codeACMDD_step06_docking_6CM4.ipynb`–`src/Group6_codeACMDD_step07_unwanted-substructures.ipynb` | 3.12.15 |

The `src/Group6_codeACMDD_step04_ml.ipynb` saved package-install output records these package
versions: RDKit 2026.3.6, scikit-learn 1.9.1, joblib 1.6.0, tqdm 4.70.0,
Matplotlib 3.11.1, pandas 3.0.6, NumPy 2.5.3, and SciPy 1.18.1. This is
captured setup output, not a complete lockfile for every notebook.

The available docking environment was queried on 2026-10-18 and reports:

| Component | Version |
| --- | --- |
| Python | 3.12.15 |
| AutoDock Vina | 1.2.7 |
| Meeko | 0.8.0 |
| RDKit | 2026.03.1 |
| Biopython | 1.88 |
| PDBFixer | 1.12 |
| OpenMM | 8.6.1 |

This is a snapshot of the available environment, not proof that all committed
historical results were generated with these exact versions. New runs should
use the pinned `requirements.txt` for the core analysis and the versioned
Conda setup in [`docking/README.md`](../docking/README.md) for docking.

## Scope of implementation

The repository contains the seven numbered analysis notebooks described in
the top-level README. The checked-in docking workflow covers single-ligand
docking and batch docking of the top 100 predictions in each ZINC22 set. It
does not implement or include the report's risperidone redocking, docking of
CHEMBL2016623, dopamine, ulotaront, or ethane controls; selection and
redocking of five hits; Asp114 analysis; molecular dynamics; or 9BS9 and
5-HT2C receptor comparisons. Those analyses require separate code, inputs,
and outputs before they can be described as reproducible from this repository.

The available scaffold collection is the result of a similarity search. A
report should call it a scaffold-similarity search unless scaffold novelty is
demonstrated with explicit structural comparisons; the search label alone does
not establish scaffold hopping.
