---
title: 3. QM inputs
layout: default
parent: "How to use RINRUS"
nav_order: 5
---

# Generating input files

Use `write_input.py` to generate input files for quantum chemistry packages. Currently input files can be created for Gaussian, xTB (through Gaussian), ORCA and Q-Chem. Some of the options for this script are specific to our QM-cluster modelling workflow and may not be relevant to other users.

**Make sure to use a model pdb file that has the frozen atom info encoded (see template pdb section above) to ensure the atom constraints are applied properly**

Basic usage of `write_input.py` to create an input file to do a basic geometry optimisation + frequency calculation:
```bash
# Example usage of write_input: Gaussian
python3 $HOME/git/RINRUS/bin/write_input.py -pdb model_N_template.pdb -c 2 -format gaussian

# Example usage of write_input: xTB (in Gaussian)
python3 $HOME/git/RINRUS/bin/write_input.py -pdb model_N_template.pdb -c 2 -format gau-xtb

# Example usage of write_input: ORCA
python3 $HOME/git/RINRUS/bin/write_input.py -pdb model_N_template.pdb -c 2 -format orca

# Example usage of write_input: Q-Chem
python3 $HOME/git/RINRUS/bin/write_input.py -pdb model_N_template.pdb -c 2 -format qchem

# Useful arguments for basic usage of write_input:
-pdb FILE           pdb file to use as structure in input file
-m MULT             model multiplicity
-c LIGCHRG          ligand charge
-format PROG        software package (gaussian/gau-xtb/orca/qchem)
-intmp FILE         input template for selected software package (if none specified, the ones in $HOME/git/RINRUS/template_files/ are used)
-inpn NAME          name of input file (default: 1.inp)
-basisinfo 'intmp' (Gaussian only) use basis set info from the input template file instead of the library in $HOME/git/RINRUS/lib3/gaussian_basis_dict.py
```
<br> 

Full set of arguments for using `write_input.py`:
```bash
# General arguments:
-m MULT            model multiplicity
-c LIGCHRG         ligand charge
-format PROG       software package (gaussian/gau-xtb/orca/qchem/psi4-fsapt)
-intmp FILE        input template for selected software package (defaults to the ones in $HOME/git/RINRUS/template_files/ if none specified)
-inpn NAME         name of input file (default: 1.inp or input.dat for psi4-fsapt)
-basisinfo 'intmp' (Gaussian only) use basis set info from the input template file instead of default RINRUS basis set library
-wdir PATH         working directory
-type TYPE         type of structure processing for input file:
                   'pdb': use pdb file normally (default)
                   'hopt': freeze all heavy atoms so only hydrogen atoms optimized
                   'gauout': take structure from Gaussian output
                   'replacecoords': update selected atom coords e.g. to create TS guess

# If using -type 'pdb' (default if no type specified) or 'hopt', these are required:
-pdb FILE          pdb file

# If using -type 'gauout', these are required:
-tmp FILE          template pdb file for creating new pdb from output
-outf FILE         Gaussian output file
-ckp FILE          Gaussian checkpoint file

# If using -type 'replacecoords', these are required:
-pdb1 FILE         starting pdb file
-pdb2 FILE         pdb to take coordinates from
-parts FILE        text file containing species in pdb1 to be replaced with pdb2 coordinates

# If using -format 'psi4-fsapt', these are required:
-seed SEED         seed to define as fragment A, rest of enzyme will be fragment B
```

# Once calculations are complete

- [Convert optimised geometries back to PDB format with the template pdb](Struc_conversion.html)
- [Analyse F-SAPT outputs](FSAPT.html)