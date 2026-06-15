---
title: define_fA_fB
layout: default
parent: "Individual functions"
---

# define_fA_fB.py

## Help message
```
usage: define_fA_fB.py [-h] [-pdb PDBF] [-s SEED] [-inp INPF]

Create fA.dat and fB.dat files for FSAPT analysis of all FGs at once

options:
  -h, --help      show this help message and exit
  -pdb PDBF       model pdb
  -s, -seed SEED  seed, examples: A:300,A:301,A:302
  -inp INPF       input file for fsapt calculation
```

## Inputs

- `-pdb`: Processed PDB
  - Given as filename
  - Example: `-pdb 2cht_h.pdb`
- `-s`: Seed
  - Given as ch:ID pairs
  - Examples: `-s A:203` or `-seed A:301,A:302`
- `-inp`: fsapt input file
  - Given as filename
  - Example `-inp input.dat`

## Outputs

- `fA.dat` and `fB.dat` which define functional groups in the two fragments