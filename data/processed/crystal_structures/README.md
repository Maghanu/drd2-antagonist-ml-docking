# Processed DRD2 crystal structures

`6CM4_Na.mmcif` is a separately processed DRD2 structure. It derives from
6CM4 and differs from the unmodified source: one Na+ ion was artificially
added at (5.6091423, 2.42903519, 3.75315261) angstrom, and crystallographic
oleic-acid molecules (component `OLA`) were omitted. **It is not used by the
current docking notebook.**

The current workflow instead downloads 6CM4 from RCSB and extracts only
DRD2 `ATOM` records, excluding `HETATM` records and the T4 lysozyme fusion.
The notebook uses co-crystallized risperidone from that downloaded CIF to
define the docking box. Keep this separate processed file and the raw
structure unchanged, and do not describe either as the receptor input to the
current notebook.
