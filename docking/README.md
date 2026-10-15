# Molecular docking

This folder is reserved for a reproducible AutoDock Vina workflow. No receptor
or ligand files and no docking runs are included yet.

## WSL setup

1. Install WSL 2 with an Ubuntu distribution and keep project files in the
   WSL filesystem for docking runs where practical.
2. In Ubuntu, install and verify Vina:

   ```bash
   sudo apt update
   sudo apt install autodock-vina
   vina --version
   ```

   If the Ubuntu package is unavailable or does not meet the protocol's
   requirements, install a documented Vina release using its official
   instructions and record the exact version.
3. Prepare the receptor and ligands using a documented, versioned preparation
   workflow. Confirm protonation, atom types, charges, missing residues,
   cofactors, waters, and ligand stereochemistry. Do not treat a PDB file as a
   prepared Vina receptor without checking its preparation.
4. Save prepared receptor files under `receptor/`, ligand inputs under
   `ligands/`, and a per-run configuration and all outputs under
   `runs/<run-id>/`. Keep `runs/docking_summary.csv` as the aggregated result
   table.

For each receptor, record the source structure (for example, PDB ID 6CM4 when
appropriate), chain, construct, preparation choices, and file provenance.
For each run, preserve the Vina version, configuration including search-box
center and size, random seed, input checksums, command, and output. Validate
the protocol on a known reference ligand before screening candidates. Docking
scores are prioritization signals, not experimental binding measurements.
