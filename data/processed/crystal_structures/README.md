# Processed crystal structures

`6CM4_Na.mmcif` is the DRD2 receptor structure used for docking. It is derived
from 6CM4 and has one Na+ ion artificially added at (5.6091423, 2.42903519,
3.75315261) angstrom. This ion is not present in the original 6CM4 crystal structure;
the unmodified structure is preserved at `data/raw/crystal_structures/6cm4.mmcif`.

The processed structure also has the crystallographic oleic-acid molecules
(component `OLA`) omitted. The docking notebook extracts only the DRD2
sequence regions from this structure, omitting the T4 lysozyme fusion from
the receptor used for docking.

The docking notebook uses this processed file for receptor coordinates and
the raw 6CM4 file only for the co-crystallized risperidone coordinates that
define the docking box. Preserve the raw structure unchanged.
