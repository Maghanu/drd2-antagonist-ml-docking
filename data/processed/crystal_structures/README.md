# Processed DRD2 crystal structures

`6CM4_Na.mmcif` is the receptor input to `src/06_docking.ipynb`. It derives
from 6CM4 and differs from the unmodified source: one Na+ ion was artificially
added at (5.6091423, 2.42903519, 3.75315261) angstrom, and crystallographic
oleic-acid molecules (component `OLA`) were omitted. The notebook retains the
DRD2 protein atoms, repairs and minimizes the protein, then includes the
processed file's Na+ ion in the final Vina receptor PDBQT.

The processed CIF omits co-crystallized risperidone. The notebook reads the
local raw `data/raw/crystal_structures/6cm4.mmcif` only for the risperidone
coordinates used to define the docking box. Neither file is downloaded at
runtime. Keep both files unchanged; document any future processing separately.
