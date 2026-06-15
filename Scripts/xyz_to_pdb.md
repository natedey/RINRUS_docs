---
title: xyz_to_pdb
layout: default
parent: "Individual functions"
---

# xyz_to_pdb.py

## Usage and arguments
```
usage: xyz_to_pdb.py [-h] [-xyz XYZ] [-pdb PDBF] [-frame FRAME] [-name NAME]

generate pdbfiles from orca xyz file

options:
  -h, --help    show this help message and exit
  -xyz XYZ      xyz structure file
  -pdb PDBF     template pdb file
  -frame FRAME  select frame: -1 (final=default) or integer (count starts at 0) or "all"
  -name NAME    filename for output pdb
```

## Inputs

- `-xyz`: xyz file to extract geometry from (required)
  - Given as filename, default is `orca.xyz`
  - Example: `-xyz orca.xyz`
- `-pdb`: Template pdb to put optimised geometry into (required)
  - Given as filename
  - Example: `-pdb model_N_template.pdb`
- `-frame`: Which structure from the output to convert
  - Given as integer or 'all' or -1 for just the final structure (default)
  - Examples: `-frame all` or `-frame 10`
- `-name`: base name to use for converted structure
  - Given as string (with or without ".pdb" ending), default is basename of given xyz file (or 'orca' if xyz argument also left unspecified)

  - Examples: `-name model_13_ts_opt` or `-name model_13_reactant_opt.pdb`

## Outputs
- `[name].pdb` if making only final frame, `[name].f[num].pdb` if using `-frame all` or `-frame [num]`