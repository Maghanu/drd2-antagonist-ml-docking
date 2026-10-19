# Raw DRD2 crystal structures

This folder preserves source crystal-structure files unchanged. The docking
notebook reads the local PDBe `6cm4.mmcif` only to obtain co-crystallized
risperidone coordinates for defining the docking box. The receptor atoms and
Na+ ion come from `data/processed/crystal_structures/6CM4_Na.mmcif`; the raw
file is not downloaded or used as the receptor.

Keep repaired, protonated, ion-modified, or otherwise prepared structures in
the processed-data or docking-preparation locations, never over the raw
structure. When adding or replacing a source structure, document its source,
PDB ID, retrieval date, URL, and checksum. The retrieval date for the
currently stored PDBe file is not recorded in this repository.
