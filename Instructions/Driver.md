---
title: Driver
layout: default
parent: "How to use RINRUS"
nav_order: 2
---

# Using the RINRUS driver

The driver runs the entire RINRUS model building procedure from initial PDB structure to input file with one command (and potentially a few interactive inputs along the way). The structure pre-processing needs to be done before using the driver (except for protonation with reduce, although we strongly recommend doing that beforehand as well so the results can be checked).

## Calling the driver:

```bash
# Usage of the RINRUS driver
python $HOME/git/RINRUS/bin/RINRUS_driver.py 

# Optional arguments:
-i FILE    rinrus driver input file (default: rinrus.inp)
``` 

## Input file

The input file is a simple text file containing "keyword: value" lines that tell the driver which model building steps to run and what arguments to pass to the relevant scripts. 
All arguments for the individual scripts can be defined in the driver input file. A [template `rinrus.inp` file](https://github.com/natedey/RINRUS/blob/master/template_files/rinrus.inp) containing all recognized keywords is provided in the template_files directory. 

### Syntax

The input file syntax is simply a set of "keyword: value" lines, for example:
```
pdb: 2cht_h_corrected_new.clean.pdb
seed: A:203
rin_program: probe
model: max
model_prot_ignore_ids: A:203
qm_input_format: orca
seed_charge: -2
```

### Recognized keywords

The `rinrus.inp` file must contain the following options (only one of model and approx_model_size_limit required, approx_model_size_limit used if both are given):

| keyword | value | function argument defined |
|:--------|:------|:---------|
| PDB | starting PDB filename | `-pdb` for most functions |
| seed | ch:ID identifiers | `-s` for most functions |
| RIN_program | probe/arpeggio/distance/manual | |
| model | all/max/maximal/[integer] | `-model` for [rinrus_trim2_pdb.py](../Scripts/rinrus_trim2_pdb.html) |
| approx_model_size_limit | [integer] | `-approx_model_size_limit` for [rinrus_trim2_pdb.py](../Scripts/rinrus_trim2_pdb.html) |
| qm_input_format | gaussian/gau-xtb/orca/qchem/psi4-fsapt/none | `-format` for [write_input.py](../Scripts/write_input.html) |


The following optional keywords can also be read from `rinrus.inp`:

| keyword | value | function argument defined |
|:--------|:------|:---------|
| path_to_scripts | path to RINRUS bin directory | |
| protonate_initial | true/t/y or false/f/n | |
| res_atoms_file | res_atoms file to use with "RIN_program: manual" | `-ra` for [rinrus_trim2_pdb.py](../Scripts/rinrus_trim2_pdb.html) |
| arpeggio_proximal | true/t/y or false/f/n | `-prox` for [arpeggio2rins.py](../Scripts/arpeggio2rins.html) |
| arpeggio_rank | counts/types | `-ra` for [rinrus_trim2_pdb.py](../Scripts/rinrus_trim2_pdb.html) |
| dist_type | closest/mass/avg | `-type` for [dist_rank.py](../Scripts/dist_rank.html) |
| dist_satom | seed atoms for distance calculations | `-satom` for [dist_rank.py](../Scripts/dist_rank.html) |
| dist_max| distance cutoff in Angstroms | `-max` for [dist_rank.py](../Scripts/dist_rank.html) |
| dist_noH | true/t/y or false/f/n | `-noH` for [dist_rank.py](../Scripts/dist_rank.html) |
| dist_byres | true/t/y or false/f/n | `-byres` for [dist_rank.py](../Scripts/dist_rank.html) |
| must_add | ch:ID[:S/C/N] of non-seed fragments that need to be in model | `-mustadd` for [rinrus_trim2_pdb.py](../Scripts/rinrus_trim2_pdb.html) |
| unfrozen | ch:ID[:CA/CB] of atoms to avoid constraining | `-unfrozen` for [rinrus_trim2_pdb.py](../Scripts/rinrus_trim2_pdb.html) |
| nc_res_info | non-canonical residue info file | `-ncres` for [rinrus_trim2_pdb.py](../Scripts/rinrus_trim2_pdb.html) |
| model_prot_ignore_ids | ch:ID identifiers | `-ignore_ids` for [pymol_protonate.py](../Scripts/pymol_protonate.html) |
| model_prot_ignore_atoms | ch:ID:atom identifiers | `-ignore_atoms` for [pymol_protonate.py](../Scripts/pymol_protonate.html) |
| model_prot_ignore_atnames | atom names | `-ignore_atnames` for [pymol_protonate.py](../Scripts/pymol_protonate.html) |
| QM_input_template | path to input template | `-intmp` for [write_input.py](../Scripts/write_input.html) |
| Gaussian_basis_intmp | true/t/y or false/f/n | `-basisinfo` for [write_input.py](../Scripts/write_input.html)
| QM_input_hopt | true/t/y or false/f/n | `-type hopt` for [write_input.py](../Scripts/write_input.html)
| seed_charge | charge of seed fragment(s) | `-c` for [write_input.py](../Scripts/write_input.html)
| multiplicity | total spin multiplicity | `-m` for [write_input.py](../Scripts/write_input.html)
| fsapt_fA | ch:ID identifiers | `-seed` for [write_input.py](../Scripts/write_input.html)



## Log file
The driver writes a log file called `rinrus_log_[date].out` which contains the details of the run:
- Header containing the github version tag so that building procedure can be reproduced even if code is later changed
- Python executable being used (for troubleshooting errors with python packages)
- The raw contents of the driver input file
- The options selected from parsing the driver input file
- The commands run by the driver in each step of the model building procedure