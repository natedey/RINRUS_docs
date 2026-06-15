---
title: fisapt_analysis
layout: default
parent: "Individual functions"
---
# fisapt_analysis.py

This script runs the Psi4 FSAPT analysis script, sorts the functional groups by absolute interaction energy, and then prepares a res_atoms.dat file for sequential model building based on the F-SAPT energies. 

## Usage and arguments
```
usage: fisapt_analysis.py [-h] [-path PATH] [-save SAVEPATH] [-rank RANK]

FSAPT analysis, summary and res_atoms creation

options:
  -h, --help      show this help message and exit
  -path PATH      path to PSI4 fsapt.py script
  -save SAVEPATH  location for saving sorted summary file and res_atoms.dat
  -rank RANK      seed part to use for Eint ranking ("whole" or one of the labels used in fA.dat)
```

## Inputs

- `-path`: path to the folder containing the PSI4 fsapt.py script
  - Given as path (to folder, not the fsapt.py script itself)
  - By default, script determines the path from the location of your psi4 executable. This is mainly still here in case you've installed it in a non-standard way or something
  - Example: `-path ~miniconda3/envs/psi4/share/psi4/fsapt`
- `-save`: location to save ranked FGs table and res_atoms_fsapt.dat file
  - Given as path, default is `../` so that files are in the same directory as the original calculation input file
  - Example: `-save ../`
- `-rank`: specify what part of fA to use for ranking the |Eint| values
  - Given as one of the functional group labels from `fA.dat` or 'whole', default is 'whole'
  - The default partitioning of fA in [define_fA_fB.dat](define_fA_fB.html) is by residue IDs, but users may manually separate these further
  - Examples `-rank A:300`


## Outputs

- `FG-SAPT-ranked.dat`: the partitioned interaction energies, ranked by the part of the seed specified with `-rank`
- `res_atoms_fsapt.dat`: res_atoms.dat file for making a set of sequential models with the ranking in `FG-SAPT-ranked.dat`