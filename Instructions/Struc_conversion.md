---
title: Output geometry conversion
layout: default
parent: "How to use RINRUS"
nav_order: 6
---

# Converting optimized structures back to the constraint-encoded PDB format

Keeping structures in the RINRUS constraint-encoded PDB format makes it easier to manage the frozen atoms.

Use [`gopt_to_pdb.py`](../Scripts/gopt_to_pdb.md) or [`xyz_to_pdb.py`](../Scripts/xyz_to_pdb.md) to convert coordinates from QM outputs into pdb format. 
```bash
# Example usage of gopt_to_pdb
python3 $HOME/git/RINRUS/bin/gopt_to_pdb.py -gout 1.out -pdb model_N_template.pdb -name model_N_opt

# Example usage of xyz_to_pdb
python3 $HOME/git/RINRUS/bin/xyz_to_pdb.py -xyz orca.xyz -pdb model_N_template.pdb -name model_N_opt

# Arguments for gopt_to_pdb/xyz_to_pdb
-gout OUTFILE   Gaussian output file (gopt_to_pdb only)
-xyz XYZFILE    XYZ file (xyz_to_pdb only)
-frame FRAME    geometry to use: -1 for final (default), integer or "all". NOTE: COUNT STARTS AT 0.
-name SAVENAME  name for new pdb file (defaults: "gopt" or base name of specified XYZ file)
```
If making only final frame, structures are saved as `[name].pdb`. If option for -frame is "all" or an integer, file(s) will be called `[name].f[num].pdb`.