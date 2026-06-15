---
title: make_template_pdb
layout: default
parent: "Individual functions"
---

# make_template_pdb.py

This script adds constraint flags to a modified PDB file. Main use is after the initial model has been capped as PyMOL removes the non-standard constraint column.

The template PDB is designed to simplify the computational workflow by taking constraint application/management off the user's hands. 

## Help message
```
usage: make_template_pdb.py [-h] [-model MODNAME] [-noh NO_H_PDB] [-addh H_ADD_PDB]

Prepare template PDB files

options:
  -h, --help       show this help message and exit
  -model MODNAME   model number
  -noh NO_H_PDB    trimmed_pdb_file
  -addh H_ADD_PDB  hadded_pdb_file
```

## Inputs

Inputs are an initial model PDB file containing constraint flags and a subsequently modified model PDB file without constraint flags. These can be specified in one of two ways. 

Specifiying by model:
- `-model`: model number to make template PDB for
  - Given as integer
  - Will define the initial PDB and modified PDB as `res_N.pdb` and `res_N_h.pdb` for given model label N
  - Example: `-model 10`

Specifying the two PDBs separately:
- `-noh`: initial PDB with constraints
  - Given as filename
  - Example: `-noh res_N.pdb`
- `-addh`: modified PDB without constraints
  - Given as filename
  - Example: `-addh res_N_h.pdb`

## Outputs

- `model_N_template.pdb`, a constraint-encoded model PDB file