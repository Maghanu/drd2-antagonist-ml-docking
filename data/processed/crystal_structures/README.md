# Processed DRD2 crystal structures

`6CM4_Na.mmcif` is the processed DRD2 structure used as the receptor source
for the project's AutoDock Vina workflow in
[`src/06_docking.ipynb`](../../../src/06_docking.ipynb). It derives from 6CM4
and differs from the unmodified source: one Na+ ion was artificially added at
(5.6091423, 2.42903519, 3.75315261) angstrom, and crystallographic oleic-acid
molecules (component `OLA`) were omitted.

The structure contains the T4 lysozyme fusion in the experimental construct;
the docking notebook extracts the DRD2 regions and omits the fusion when
preparing the receptor. The raw 6CM4 file at
`data/raw/crystal_structures/6cm4.mmcif` is used for the co-crystallized
risperidone coordinates that define the docking box. Keep the raw structure
unchanged and document further processing separately.
