---
title: pymol_protonate
layout: default
parent: "Individual functions"
---

# pymol_protonate.py

## Help message
```
usage: pymol_protonate.py [-h] [-pdb PDB [PDB ...]] [-ignore_ids IGNORE_IDS] [-ignore_atoms IGNORE_ATOMS] [-ignore_atnames IGNORE_ATNAMES]

options:
  -h, --help            show this help message and exit
  -pdb PDB [PDB ...]
  -ignore_ids IGNORE_IDS
  -ignore_atoms IGNORE_ATOMS
  -ignore_atnames IGNORE_ATNAMES
```

## Inputs

- `-pdb`: Trimmed PDB to be protonated (required)
  - Given as filename (res_N.pdb)
  - Example: `-pdb res_10.pdb`
- `-ignore_ids`: fragment(s) to exclude from protonation
  - Given as ch:ID pairs
  - We recommend excluding the seed/anything that has manually been protonated in a specific way
  - Examples: `-ignore_ids A:203` or `-ignore_ids A:301,A:302`
- `-ignore_atoms`: specific atom(s) to exclude from protonation
  - Given as ch:ID:atom[+atom[+atom...]]
  - More specific control for protecting manually protonated stuff, e.g. keeping one atom of a side chain unprotonated when you still need CA to be capped
  - Examples: `-ignore_atoms A:203:C1` or `-ignore_ids A:203:C5+O5` etc
- `-ignore_atnames`: specific atom type(s) to exclude from protonation
  - Given as atom names, e.g. "ND1,NE2" to avoid PyMOL changing any histidine protonation states
  - Be careful with this!!
  - Example: `-ignore_atnames ND1,NE2`

## Outputs

- `res_N_h.pdb` for input `res_N.pdb`: capped PDB file, will not have the frozen atom column from `res_N.pdb`
- `log.pml`: pymol script used to run the protonation. If `pymol_protonate.py` can't run pymol for whatever reason, the user can run this in pymol manually