# Data provenance

Keep source exports unchanged in `data/raw/`. Put cleaned activity tables,
standardized structures, and derived fingerprints in `data/processed/`.
Never silently replace a source file; if a new export is retrieved, retain its
retrieval details and distinguish it from earlier versions.

Store unmodified experimental crystal structures under
`data/raw/crystal_structures/`. Keep repaired, protonated, or otherwise edited
structures outside the raw-data folder and document their derivation.

## Planned source

- **Source:** ChEMBL (https://www.ebi.ac.uk/chembl/)
- **Target:** DRD2; record the exact ChEMBL target identifier and target
  definition when selecting records.
- **Download date:** Not downloaded yet.
- **Raw export:** None present yet.
- **Filters applied:** None yet. Define and record assay type, activity
  relation, units, confidence/target assignment, duplicate handling, and
  structure-quality rules before generating curated tables.

Update this file when data are acquired or filters change. For each raw export,
record its filename, source query or API request, retrieval date, and checksum.
For each processed table, describe transformations, excluded-record counts,
activity-unit conversions, and the script/version that produced it. Preserve
original measurement values and units alongside converted values where
possible.

Do not infer antagonism from binding potency alone: retain assay type and
mechanism annotations, and document how labels are assigned.
