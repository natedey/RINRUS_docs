---
title: dist_rank
layout: default
parent: "Individual functions"
---

# dist_rank.py

## Usage and arguments
```
usage: dist_rank.py [-h] [-pdb PDBF] [-s SEED] [-satom SEEDATOM] [-type DTYPEF] [-max COFF] [-noH] [-store] [-recut] [-byres]

Distance based selection scheme with both residue and functional group partitioning

options:
  -h, --help           show this help message and exit
  -pdb PDBF            pdb file to use
  -s SEED, -seed SEED  seed, examples: A:300,A:301,A:302
  -satom SEEDATOM      atoms to use as center, examples: A:300:CA
  -type DTYPEF         "closest" or "mass" or "avg"
  -max COFF            cutoff/max distance from ligand, default 5A
  -noH, -noh           ignore hydrogens
  -store               store distances and pdb/seed/noH info in all_dists.pkl
  -recut               apply new cutoff to atom distances read from all_dists.pkl
  -byres               use residue grouping instead of FG grouping
```

## Inputs

- `-pdb`: Processed PDB to be trimmed (required)
  - Given as filename
  - Example: `-pdb 2cht_h.pdb`
- `-s`: Seed
  - Given as ch:ID pairs
  - Examples: `-s A:203` or `-seed A:301,A:302`
- `-satom`: Specific atoms to use as distance center instead of whole seed
  - Given as ch:ID:atom trios
  - Examples: `-satom A:203:CA` or `-satom A:301:CA,A:302:CA`
- `-type`: type of distance to calculate
  - Given as "closest" or "mass" or "avg"
  - closest: distance to closest seed atom
  - mass: distance to center of mass of seed/satom
  - mass: distance to Cartesian center of seed (average of seed coordinates)
  - Examples: `-type closest`
- `-max`: distance cutoff
  - Given as number (in Å)
  - Example: `-max 5`
- `-noh`/`-noH`: flag to specify that hydrogens should be ignored in distance calculations
  - No corresponding argument (acts as t/f flag)
  - Hydrogens will also be excluded from calculation of the center of mass/Cartesian center
  - Examples: `-noh`
- `-store`: save distances of all atoms to seed so that new cutoff can be applied faster
  - No corresponding argument (acts as t/f flag)
  - saves info to `all_dists.pkl`
  - Example: `-store`
- `-recut`: use previously stored `all_dists.pkl` instead of calculating distances from scratch
  - No corresponding argument (acts as t/f flag)
  - Example: `-recut`
- `-byres`: do distance calculations by residue rather than functional group
  - No corresponding argument (acts as t/f flag)
  - Examples: `-byres`

## Outputs

- `sorted_FGs_[type]_[max]_A.dat` or `sorted_res_[type]_[max]_A.dat`: ranked FGs or residues up to given cutoff
- `all_dists_table.dat`: distances of all atoms in PDB to seed
- `res_atoms.dat`
