---
title: write_input
layout: default
parent: "Individual functions"
---

# write_input.py

This script prepares input files for QM computations in the Gaussian, ORCA, Q-Chem and Psi4 (limited options) software packages.

## Help message
```
usage: write_input.py [-h] [-type STEP] [-m MULTIPLICITY] [-c LIGAND_CHARGE] [-format FMAT] [-intmp INPUT_TMP] [-inpn INP_NAME] [-basisinfo BASISINFO] [-wdir OUTPUT_DIR] [-pdb NEW_PDB] [-tmp TMP_PDB] [-gout GAU_OUT] [-ckp CHECK_POINT] [-pdb1 PDB1] [-pdb2 PDB2] [-parts PARTS] [-fA SEED]

Prepare input files

options:
  -h, --help            show this help message and exit
  -type STEP            hopt: read pdb and write input with heavy atoms constrained,
                        gauout: read gaussian output and write new input with final structure,
                        pdb: read pdb and write input
                        replacecoords: read pdb1 and pdb2, put pdb2 coords into pdb1 for atoms specified in parts.txt, write input and save input_parts_replaced.pdb
                        fsapt: read pdb and write input for F-SAPT0 calculation (psi4 only, overwrites -format)
  -m MULTIPLICITY       multiplicity
  -c LIGAND_CHARGE      charge of ligand
  -format FMAT          input file format eg.'gaussian','qchem','gau-xtb','orca','psi4-fsapt'
  -intmp INPUT_TMP      input file template (uses ones in RINRUS/templates/ if not specified)
  -inpn INP_NAME        input name
  -basisinfo BASISINFO  'intmp' if in template file, use dictionary otherwise
  -wdir OUTPUT_DIR      working dir
  -pdb NEW_PDB          model pdb file
  -tmp TMP_PDB          template pdb file
  -gout GAU_OUT         gaussian output
  -ckp CHECK_POINT      check file frame
  -pdb1 PDB1            model pdb file
  -pdb2 PDB2            pdb with new coords
  -parts PARTS          parts.txt listing atoms to replace
  -fA SEED              fragment A for F-SAPT calc
```

## Inputs

General inputs:
- `-m`: multiplicity, default 1
- `-c`: charge of ligand/non-standard fragments, default 0
  - This needs to be the total charge of all non-standard fragments/anything that does not have its charges included in the PDB file
  - write_input gets the charges of standard amino acids from the PDB file (columns 79-80 of the PDB format) and adds them to the given charge to calculate the overall charge of the system that is put into the input file
- `-format`: which computational program to prepare input for
  - recognized options: gaussian, gau-xtb, orca, q-chem, psi4-fsapt
- `-intmp`: input file template to get calculation details from
  - if not specified, uses ones in the [template directory](https://github.com/natedey/RINRUS/tree/master/template_files)
- `-basisinfo intmp`: flag that the basis set should be read from the input template (for gaussian inputs only)
  - if not specified, uses [inbuilt gaussian basis set dictionary](https://github.com/natedey/RINRUS/blob/master/lib3/gaussian_basis_dict.py)
- `-inpn`: name for input file
  - default is 1.inp for gaussian/gau-xtb/orca/q-chem inputs and input.dat for psi4 inputs. if making all models with the driver, input files will be named model_N.inp to make it clear which is for each model
- `-wdir`: working directory (where to make the inputs), default is current dir

Other arguments depend on which option for the `-type` argument is selected. The type argument determines how the geometry and constraints will be determined when writing the input file. 

- `-type pdb`: read geometry and constraints directly from PDB file. the default option if no type argument given
  - geometry then specified with `-pdb` argument
- `-type hopt`: read geometry from PDB file and constrain all hydrogen atoms
  - geometry then specified with `-pdb` argument
- `-type gauout`: read geometry and constraints from gaussian output file
  - gaussian output file specified with `-gout`

- `-type replacecoords`: replace part of pdb1 structure with geometry from pdb2, use constraints from pdb1
  - model pdbs specified with `-pdb1` and `-pdb2`. 
- `-pdb`, `-pdb1`, `-pdb2`: QM-cluster model structure files to make input for
  - use `-pdb` with type = pdb/hopt/fsapt
  - use `-pdb1` AND `-pdb2` for type = replacecoords
- `-parts`: file specifying which atoms 



## Outputs

- QM input file: called `1.inp` if making one model, `model_N.inp` if making sequence with driver, `input.dat` for FSAPT input files, or any input name specified with `-inpn` argument
- If using `-type replacecoords`, the modified geometry is saved as `input_parts_replaced.pdb`