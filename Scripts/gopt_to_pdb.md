---
title: gopt_to_pdb
layout: default
parent: "Individual functions"
---
# gopt_to_pdb.py

## Help message
```
usage: gopt_to_pdb.py [-h] [-gout OUTPUT] [-pdb PDBF] [-frame FRAME] [-name NAME]

generate pdbfiles from 1.out

options:
  -h, --help    show this help message and exit
  -gout OUTPUT  gaussian output file
  -pdb PDBF     template pdb file
  -frame FRAME  select frame: -1 (final=default) or integer or "all"
  -name NAME    filename for output pdb
```

## Inputs

- `-gout`: Gaussian output file to extract geometry from (required)
  - Given as filename, default is `1.out`
  - Example: `-gout file.out`
- `-pdb`: Template pdb to put optimised geometry into (required)
  - Given as filename
  - Example: `-pdb model_N_template.pdb`
- `-frame`: Which structure from the output to convert
  - Given as integer or 'all' or -1 for just the final structure (default)
  - Examples: `-frame all` or `-frame 10`
- `-name`: base name to use for converted structure
  - Given as string (with or without ".pdb" ending), default is 'gout'

  - Examples: `-name model_13_ts_opt` or `-name model_13_reactant_opt.pdb`

## Outputs
- `[name].pdb` if making only final frame, `[name].f[num].pdb` if using `-frame all` or `-frame [num]`