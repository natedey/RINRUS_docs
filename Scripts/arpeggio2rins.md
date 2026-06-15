---
title: arpeggio2rins
layout: default
parent: "Individual functions"
---

# arpeggio2rins.py

`arpeggio2rins.py` analyses an Arpeggio output file, extracts all contacts involving any seed atoms, and prepares `res_atoms.dat` files for the model trimming.

## Help message
```
usage: arpeggio2rins.py [-h] [-pdb PDB] [-f ARPFILE] [-s SEED] [-prox]

Generate interaction information from arpeggio file. Usage: arpeggio2rins.py -pdb pdb -f probe_file -s seed [-prox]

options:
  -h, --help           show this help message and exit
  -pdb PDB             pdb file
  -f ARPFILE           arpeggio contacts file
  -s SEED, -seed SEED  seed for selecting RIN, in the format of "A:300,A:301,A:302"
  -prox, -proximal     include proximal interactions
```

## Inputs
- `-pdb`: PDB file
  - Given as filename
  - Examples: `-pdb 2cht_h.pdb`
- `-f`: Arpeggio contacts file
  - Given as filename
  - Example: `-f 2cht_h.contacts`
- `-s`: Seed
  - Given as ch:ID pairs
  - Examples: `-s A:203` or `-seed A:301,A:302`
- `-prox`/`-proximal`: flag to specify that proximal interactions should be included
  - No corresponding argument (acts as t/f flag)
  - Examples: `-prox`

## Outputs
- `FG_arpeggio_counts.dat`: table listing arpeggio contact counts with seed for each FG
- `res_atoms.dat`: res\_atoms file ranked by total contact count
- `res_atoms_types.dat`: res\_atoms file ranked by number of different contact types